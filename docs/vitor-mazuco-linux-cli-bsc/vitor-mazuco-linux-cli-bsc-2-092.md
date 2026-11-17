# 092：为电商系统创建用户 👨‍💻





## 概述
在本节课中，我们将继续学习MongoDB的用户管理。我们将重点学习如何为特定的应用程序（例如一个电商系统）创建具有不同权限的用户，并配置MongoDB的安全认证。通过实践，你将掌握用户创建、角色分配以及权限验证的核心操作。


---






## 启用MongoDB安全认证 🔐



上一节我们介绍了如何创建管理员用户。本节中，我们来看看如何启用MongoDB的安全认证，这是保护数据库的第一步。




我们需要修改MongoDB的配置文件以启用安全限制、身份验证和授权。



1.  打开MongoDB的配置文件 `mongod.conf`。
2.  找到并取消注释（或添加）以下两个字段：
    ```yaml
    security:
      authorization: enabled
    ```
3.  保存并退出配置文件。
4.  重启MongoDB服务使配置生效：
    ```bash
    sudo systemctl restart mongod
    ```



重启后，MongoDB将要求身份验证才能执行操作。





---




## 以管理员身份登录并创建数据库 🗄️



现在，我们可以使用之前创建的管理员用户登录。管理员用户有权创建新用户和数据库。





使用以下命令登录，密码为 `123456`：
```bash
mongo -u admin -p 123456 --authenticationDatabase admin
```



登录后，我们为电商客户创建一个专用的数据库，命名为 `ecommerce`。
```javascript
use ecommerce
```
此时使用 `show dbs` 命令可能不会显示 `ecommerce` 数据库，因为它还没有任何集合或文档。当我们向其中添加内容后，它就会显示出来。




---


## 创建具有读写权限的电商用户 👤



接下来，我们为 `ecommerce` 数据库创建一个专属用户。这个用户将只能在该数据库内进行读写操作。




以下是创建用户的步骤和命令：
1.  切换到 `ecommerce` 数据库：`use ecommerce`
2.  执行创建用户命令：
    ```javascript
    db.createUser({
      user: "website",
      pwd: "123456",
      roles: [ { role: "readWrite", db: "ecommerce" } ]
    })
    ```
    命令执行成功后，会提示用户 `website` 已成功添加到 `ecommerce` 数据库。



现在，`website` 用户只能在 `ecommerce` 数据库中进行读取和写入操作。

---

## 验证电商用户权限 ✅




让我们退出当前会话，并使用新创建的 `website` 用户登录，以验证其权限。


退出并重新登录：
```bash
mongo ecommerce -u website -p 123456
```


登录后，尝试在 `ecommerce` 数据库中插入和查询数据：
1.  创建一个 `products` 集合并插入文档：
    ```javascript
    db.products.insertOne({ title: "MongoDB in Action" })
    ```
2.  查询插入的文档：
    ```javascript
    db.products.find()
    ```
3.  尝试更新文档：
    ```javascript
    db.products.updateOne({ title: "MongoDB in Action" }, { $set: { price: 49.99 } })
    ```
    这些操作都应该成功。


现在，尝试操作其他数据库（例如 `admin` 或 `test`），你会收到权限错误，这证明了用户权限被正确限制。


---




## 创建只读用户 📖




有时我们需要创建只能查看数据而不能修改数据的用户。让我们回到管理员账户创建一个这样的用户。




首先，用管理员重新登录：
```bash
mongo -u admin -p 123456 --authenticationDatabase admin
```


在 `admin` 数据库中创建一个全局只读用户：
```javascript
use admin
db.createUser({
  user: "reporter",
  pwd: "123456",
  roles: [ { role: "readAnyDatabase", db: "admin" } ]
})
```
这个 `reporter` 用户可以在任何数据库上执行读取操作，但无法进行任何写入或修改。



---





## 验证只读用户权限 🔍




退出并用 `reporter` 用户登录进行测试：
```bash
mongo admin -u reporter -p 123456
```

验证其权限：
1.  查看所有数据库：`show dbs`
2.  切换到 `ecommerce` 并查询数据：`db.products.find()`
3.  尝试插入新数据：
    ```javascript
    db.products.insertOne({ title: "New Book" })
    ```
    此操作会失败，并返回 **"not authorized"** 错误。这正是我们期望的只读行为。

---

## 增强管理员用户权限 ⚡

在之前的课程中，我们创建的管理员用户只能管理用户，不能直接操作数据。现在，我们为其授予更全面的权限。

使用管理员账户登录，并为其添加读写任何数据库的权限：
```javascript
use admin
db.grantRolesToUser(
  "admin",
  [ { role: "readWriteAnyDatabase", db: "admin" } ]
)
```



现在，这个 `admin` 用户既能够管理所有用户，也能够读写任何数据库中的数据，成为一个功能强大的超级用户。



---


## 创建自定义角色与多数据库权限用户 🛠️


MongoDB允许我们创建自定义角色，并为单个用户分配跨多个数据库的特定权限。这是一个非常灵活的安全功能。




例如，我们可以创建一个用户，让他在 `ecommerce` 数据库拥有全部权限，同时只能读取 `hr` 数据库中 `employees` 集合的数据。




首先，创建一个自定义角色：
```javascript
use admin
var websiteRole = {
  role: "websiteManager",
  privileges: [
    {
      resource: { db: "hr", collection: "employees" },
      actions: [ "find" ]
    },
    {
      resource: { db: "ecommerce", collection: "" },
      actions: [ "find", "insert", "update", "remove" ]
    }
  ],
  roles: []
}
db.createRole(websiteRole)
```



然后，创建一个使用这个自定义角色的用户：
```javascript
db.createUser({
  user: "master",
  pwd: "123456",
  roles: [ "websiteManager" ]
})
```
这样，`master` 用户就具备了我们在 `websiteManager` 角色中定义的跨数据库混合权限。


---



![](img/121c52d45c33a0dd746af7a44a873ed0_150.png)

## 总结
本节课中，我们一起学习了MongoDB用户管理与安全配置的核心实践。我们完成了以下内容：
1.  **启用MongoDB安全认证**，为数据库访问设置门槛。
2.  **创建特定应用数据库**及其专属用户，并限制其权限范围。
3.  **验证用户权限**，通过实际操作确认读写和只读权限是否生效。
4.  **增强用户权限**，将管理员升级为超级用户。
5.  **创建自定义角色**，实现灵活、细粒度的跨数据库权限分配。

这些技能是构建安全、可靠应用程序的基础。请务必完成课后练习，尝试创建不同的用户和角色，并观察各种权限错误，以加深对MongoDB安全机制的理解。