![GitHub Workflow Status](https://img.shields.io/github/workflow/status/vapor-community/Vii/test)

# Vii

### Currently in pre-alpha (Give it a whirl but expect to need to make adjustments)

Vii is a code generation tool loosely based on Gii from the [Yii](https://github.com/yiisoft/yii2) framework. Vii can quickly reverse engineer a database schema into [Vapor 4](https://github.com/vapor) compatible class definitions, including matching SQL data types to Swift equivalents. Currently, Vii supports Postgres and MySQL. 

## Installation
To start using Vii, clone the repo

```swift
git clone https://github.com/vapor-community/Vii
```

## Usage
cd into the directory of the recently cloned git repo and run the following command.

The basic command is
```swift
swift run Vii <database specifier>
```

There are two current options, `postgres` and `mysql`. 
So to use with Postgres DB.

```swift
swift run Vii postgres
```

## @Timestamp
Vapor's `@Timestamp` propertyWrapper uses an enum called `Trigger` which helps Fluent determine whether to update this column on creation, update or deletion. Because this is not derivable from the database columns directly, Vii makes an educated guess on what case to apply to the timestamp, this is done based on the column name.

## Optional Model Properties
Any property that is potentially `Null` in the database will be made optional in the model definition. The initializer's signature will apply them directly as none, rather than omit them eg
```swift
init(id: Int? = nil...)
```

Vii will generate all the models in the specified DB and produce an output folder within the project

### To Implement
- Implement a swift formatter to remove the visual ugliness of escape sequences and make tests and everything else more friendly
- Allow subsection of schema tables to be generated
- Allow skip command to omit comma separated list of tables
- @Timestamps, probably need an educated guess process to define when the column is updated eg - if `create` appears in column name make it `.create`. If `(update)(.+)` or `(mod)(.+)` apply `.update`
- Further support for `@propertyWrappers` as currently not all are implemented and some maybe very hard to implement robustly. I'm looking at you `@sibling`
- Makes sense to add migrations to the model so users could use their own test DB
- Improved Github workflows to test DBs on Linux with small seeded DB

### Future features
- Look to hook a CRUD builder into it, I think Twof might have something like that, essentially builds a controller and/or repository. Kinda like Spring does.

