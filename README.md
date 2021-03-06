# Cargojs

![alt tag](https://dl.dropboxusercontent.com/s/uwjsu733s54m60e/Cargo_long.png?dl=0)

Cargojs is an API for [OrientDB] operations build on top of [Express].

Cargojs support few amount of operations including BREAD at the moment and build using [Orientjs] as main driver.

  - Browse
  - Read
  - Edit
  - Add
  - Delete
  - Delete-All
  - Count
  - Query
  - Map
    - Add
    - Remove
  - By RID
    - Get
    - Update
    - Delete

## Usage

#### Installation
Install via npm.

```sh
$ npm install cargojs
```

### Configuring the client.
```js
var Cargo = require('cargojs');

var configuration = {
    host: 'localhost',
    port: 2424,
    user: 'root',
    password: 'root',
    database: 'sample'
};

app.use(Cargo.express(configuration, {

    sync: Cargo.sync.CREATEOREXISTS, // Create the database from meta data
    
    define: function (db, models) {
        models.User = db.define("User", {
            'name': Cargo.types.STRING,
            'age': Cargo.types.INTEGER
        });
        
        //more class models....
    }
}));
```

### Using Cargojs operations

#### + Browse | Return an array of records

```js
exports.browseUsers = function(req, res) {
    req.models.user.browse({
            'status': 'active'
        }).then(function(users)) {
            console.log(users);
        },
        function(error) {
            console.error(error)
        });
};
```

#### + READ | Return a single record
```js
exports.readUser = function(req, res) {
    req.models.user.read({
            'status': 'active'
        }).then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error)
        });
};
```

#### + Edit | Update reocrds
```js
exports.editUser = function(req, res) {
    req.models.user.set({
            'name': 'Ryan'
        }).edit({
            'status': 'active'
        }).then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error)
        });
};
```

#### + Add | Add a reocrd
```js
exports.addUser = function(req, res) {
    req.models.user.add({
            'name': 'Ryan',
            'lastName': 'Cooper',
            'status': 'active'
        }).then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error);
        });
};
```

#### + Delete | Delete a reocrd
```js
exports.deleteUser = function(req, res) {
    req.models.user.delete({
            'name': 'Ryan'
        }).then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error);
        });
};
```

#### + Delete-All | Delete all reocrds
```js
exports.deleteAllUser = function(req, res) {
    req.models.user.deleteAll({
            'name': 'Ryan'
        }).then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error);
        });
};
```

#### + Count | Count reocrds in class
```js
exports.countUsers = function(req, res) {
    req.models.user.count().then(function(total)) {
            console.log(total);
        },
        function(error) {
            console.error(error);
        });
};
```

### Map Links

#### + Add | Add link to record
```js
exports.linkUsers = function(req, res) {

    var client = req.cargo.RID('#1:5');
    var link = req.cargo.RID('#2:3');

    req.models.user.addList(client).map({
            'project': link
        }).then(function(updatedCount)) {
            console.log(updatedCount);
        },
        function(error) {
            console.error(error);
        });
};
```

#### + Remove | Remove link to record
```js
exports.linkUsers = function(req, res) {

    var client = req.cargo.RID('#1:5');
    var link = req.cargo.RID('#2:3');

    req.models.user.removeList(client).map({
            'project': link
        }).then(function(updatedCount)) {
            console.log(updatedCount);
        },
        function(error) {
            console.error(error);
        });
};
```

#### + Query | Custom query
```js
exports.queryUsers = function(req, res) {

    var sql = 'SELECT * FROM User'

    req.models.user.query(sql).then(function(result)) {
            console.log(result);
        },
        function(error) {
            console.error(error);
        });
};
```

#### + Select | Select columns
```js
exports.selectColumnsUsers = function(req, res) {
    req.models.user.select('name, lastName').browse({
            'status': 'active'
        }).then(function(users)) {
            console.log(users);
        },
        function(error) {
            console.error(error)
        });
};
```

#### + Limit | Limit records (only works with browse right now)
```js
exports.limitUsers = function(req, res) {
    req.models.user.limit(5).browse({
            'status': 'active'
        }).then(function(users)) {
            console.log(users);
        },
        function(error) {
            console.error(error)
        });
};
```

### Let's work with RIDs (By RID)

#### + Get | Get a record
```js
exports.getUserbyRID = function(req, res) {
    req.record.get('#12:1').then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error)
        });
};
```

#### + Update | Update a record
```js
exports.updateUserbyRID = function(req, res) {
    req.record.RID('#12:1').update({
            name: 'Ryan'
        }).then(function(use)) {
            console.log(user);
        },
        function(error) {
            console.error(error)
        });
};
```

#### + Delete | Delete a record
```js
exports.deleteUserbyRID = function(req, res) {
    req.record.delete('#12:1').then(function(user)) {
            console.log(user);
        },
        function(error) {
            console.error(error)
        });
};
```

### Version
0.5.6


### Development

Want to contribute? Great! Pull-requests are welcome

### Todo's

* Remove [Orientjs] and be standalone.
* Add more operations like Create classes | Add properties etc...
* Custom operations.

License
----
[MIT]


[OrientDB]:http://orientdb.com/
[Express]:http://expressjs.com/
[Orientjs]:https://github.com/orientechnologies/orientjs
[MIT]:https://github.com/warapitiya/Cargo/blob/master/LICENSE
