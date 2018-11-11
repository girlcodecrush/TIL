

ExpressJS

app object - denotes the Express application; can create it by calling th top-level express() function exported by the Express module;

```
var express = require('express);
var app = express();

app.get('/', (req, res) => {
  res.send('andrea says hi');
});
app.listen(3000);
```

app.method(path, cb)  - cb as in middleware callback function 

```
app.use((req,res, next) => {
  next();
})

//a router is a valide middleware
var router = express.Router();
router.get('/', (req, res, next)=>{
  next();  
})
app.use(router); 

//an Express app is valide m/w
var subApp = express();
subApp.get('/', (req, res, next) => {
  next();  
})
app.use(subApp); 
```

1.ORM(Object Relational Mapping)

- application, Database 사이를 mapping 시켜주는 도구
- the mostly used ORM in nodeJS
- Backs up PostgreSQL, MySQL, MariaDB, SQLite, MSSQL, transactions, relation, read replication
- Feature : make Promise operate basically



Pros: 

- 특정 DBMS에 종속 안 됨
- SQL문이 코드에 들어가지 않아 깔끔한 코드
- ORM이 nesting된 data를 binding 

Cons:

- raw query에 비해 느림
- Query tuning 어려움
- 서비스가 복잡할수돌 orm으로 할 수 있는 작접 범위 한계

Promise

- neat complicated asynchronous codes
- Returns a value via Chaining or process a series of works 
- error handling
- Libraries that execue Promise: Q, RSVP, bluebird

Set up:

- PostgreSQL(RDBMS) - used as db  

```
new Sequelize(database, [username = null], [password =null], [options={}])
//to connect DB to the server
var sequelize = sequelize('sequelize', null, '1234', {
  host: 'locathost',
  dialect: 'postgres'
}); 
```

Model Define

- Model은 database 공간의 table의 Schema를 표현하는 수단
- Model is defined using define() method of Sequelize

```
sequelize.define('Publisher', {  
    pub_id: {type:DatTypes.INTEGER, 
             primaryKey: true, 
             autoIncrement: true},
    name: {type: DataTypes.STRING(32), 
           allowNull: false},
    established_date: {type: DataTypes.DATE, 
                       defaultValue: DataTypes.NOW}
}, {
    classMethods: {},
    tableName: 'publisher',
    freezeTableName: true,
    underscored: true, 
    timestamps: false
});
```

- the first param is the model's name
- Second param is the information from Table Schema, that's, schema for each column of the table 
  - assigned values to look at 
    - Type: data type
    - primaryKey: define if it is a primary key or not ( default: false)
    - autoIncrement: whether or not it is SERIAL(auto increament)  (default:false)
    - allowNull: whether or not it allows the value of null (default: true)
    - Unique: option if it is unique or not - for a single-column unique, assign as true/false
      - for a multiple-column unique,  동일한 문자열을 각 column의 unique 속성에 넣어준다
    - Comment : comment on column
    - Validate: validation check option for each column 
- third param is a config option
  - Timestamps : Sequelize는 table 생성 후 자동적으로 createdAt, updatedAt columnd을 생성
    - database에 해당 table이 언제 생성되었고 가장 최근에 수정된 시간이 언제인지 추적할 수 있게 해 준다
    - 기능 끄려면 false로 설정
  - Paranoid: if true, deletedAt column to be added to Table
    - When a certain row is deleted, the data in the row is not deleted, the date of deletion is added to deletedAt
    - the row that is in deletedAt is excluded when the find() is in progress 
    - still, the data remains in the database
    - can use Paranid only when timestamp is true
  - Underscored: if true, the column name can be named in the underscore format, not in the camelCase format
  - freezeTableName: in Sequelize, table name, first param of define() method, is automatically converted. if true, auto conversion doesn't happen
  - TableName : the real table name
  - Comment: remarks on table

**IMPORT** method

- create objects that exactly match the definitions of models and store the objects

- if called several times,as they are cached inside, there won't be any issue

- when the server is on, if model is cached on the side of application, don't have to call the import method multiple times

- Models/index.js - set sequelize

- Models/publisher.js - define publisher table 

- directory / paths

  ```
  -sequelize_tutorial(~/example/sequelize_tutorial)
   - app
    - bin
     - models
      - index.js
      - publisher.js
  ```

  index.js - import 

  ```
  var fs = require('fs');
  var path = require('path');
  var Sequelize = require('sequelize');
  var sequelize = new Sequelize('postgres: )
  ```

  

## Sequelize 설치하기

**[Sequelize](http://docs.sequelizejs.com/en/v3/)**는 Node에서 가장 많이 사용되는 ORM 입니다.

**RDS로 PostgresSQL, MySQL, MariaDB, SQLite, MSSQL**을 지원하고 transaction, read replication등 다양한 기능을 제공하고 있으며. 또한 **Promise**를 기본으로 동작하기 때문에 비동기 코드를 보기좋게 작성할 수 있습니다.

실습을 위해 express-generator를 통해 Express 프로젝트를 생성후 `sequelize`와 `mysql` module을 설치합니다. (Express 프로젝트를 생성하는 부분은 생략합니다.)

```
npm install --save sequelize mysql
```

**[Sequelize Command Line Interface(CLI)](https://github.com/sequelize/cli)**를 사용하기 위해서 `sequelize-cli` module을 설치합니다.

```
npm install -g sequelize-cli
```

------

<span id='3'>

## Sequelize CLI 사용하기

**sequelize cli**를 통해서 **`migration(마이그레이션), seeder(시더), model(모델)`**의 초기 설정을 손쉽게 할 수 있습니다. 이번 포스팅에서는 **model**에 관해서 알아보겠습니다.

RDB의 테이블을 model로 정의를 하면 해당 model을 통해 데이터 처리가 가능하게 됩니다. 먼저 생성한 express 프로젝트에 sequelize cli 명령어를 통해 sequelize 설정 파일을 생성 후 model을 정의해 보겠습니다.

```
sequelize init:config --config config/sequelize.json
sequelize init:models
```

sequelize cli의 **sequelize init:config**라는 명령어로 sequelize관련 config 파일을 자동으로 생성할 수 있습니다. 아무런 옵션을 주지 않는다면 **config/config.json** 파일이 생성됩니다.

**sequelize init:models** 명령어를 통해서는 models 정의에 관련된 기본 구조를 생성할 수 있습니다.

위 2개의 명령어를 실행하면 다음과 같은 폴더와 파일이 생성됩니다.

```
├── config/
  └── sequelize.json
├── models/
  └── index.js
```

------

<span id='4'>

## Sequelize config 설정하기

sequelize cli를 통해 생성한 `config/sequelize.json`파일에 데이터베이스에 관련된 설정 값을 입력합니다.

**NODE_ENV** 에 따라 각기 다른 값을 사용하기 때문에 상황에 맞게 설정할 수 있습니다. NODE_ENV에 대해 잘 알지 못한다면 [이곳](http://inspiredjw.com/entry/Nodejs-%EC%97%90%EC%84%9C-NODEENV-%EA%B0%92%EC%9C%BC%EB%A1%9C-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)을 참고하세요.

데이터베이스를 사용한 프로젝트 경험이 있다면 대부분의 config 값은 입력할 수 있습니다. config 값중 `dialect`에는 사용하는 RDB 이름을 입력해야 합니다. diaect에 사용 가능한 값은 [sequelize docs](http://docs.sequelizejs.com/en/1.7.0/docs/usage/)를 참고하세요. 현재 사용 가능한 RDB 로는 'mysql', 'sqlite', 'postgress', 'mariadb'가 있습니다.

추가적으로 커넥션 풀과 로깅 기능을 사용한다면 해당 값을 추가합니다. 추가적으로 필요한 옵션은 docs를 참고하세요.

```
"pool": {
    "max": 20,
    "min": 0,
    "idle": 5000
},
"logging": true
```

------

<span id='5'>

## # Model 정의하기

Model을 생성하기 전 sequelize cli를 통해 생성한 `models/index.js`파일을 살펴보겠습니다. index.js 의 역할은 **config/sequelize.json**의 설정값을 읽어 sequelize를 생성한 후 **models 폴더** 아래에 정의한 model 관련 js 파일을 모두 로딩하여 db 객체에 Model을 정의한 후 반환합니다.

sequelize config 관련 파일을 sequelize.json으로 생성하였다면 config 파일을 불러오는 **require 부분의 경로를 수정**해주어야 합니다.

이제 models 폴더 아래에 간단한 모델을 정의해 보겠습니다. **`user.js`**를 생성 후 다음의 코드를 입력합니다.

```
module.exports = function (sequelize, DataTypes) {
  const user = sequelize.define('User', {
    userID: { field: 'user_id', type: DataTypes.STRING(50), unique: true, allowNull: false },
    password: { field: 'password', type: DataTypes.STRING(30), allowNull: false },
  }, {
    // don't use camelcase for automatically added attributes but underscore style
    // so updatedAt will be updated_at
    underscored: true,

    // disable the modification of tablenames; By default, sequelize will automatically
    // transform all passed model names (first parameter of define) into plural.
    // if you don't want that, set the following
    freezeTableName: true,

    // define the table's name
    tableName: 'user'
  });

  return user;
};

/*
 Sequelize 참고
 DataTypes => http://docs.sequelizejs.com/en/v3/api/datatypes/
 Associations => http://docs.sequelizejs.com/en/v3/api/associations/
 Model Function => http://docs.sequelizejs.com/en/v3/api/model/
 */
```

Model을 생성하며 사용된 옵션은 주석과 docs를 참고합니다. 이제 모델에 대한 정의가 끝났습니다. 데이터베이스에 **user**라는 테이블은 **User**라는 Object로 매핑되었고 **user_id, password**라는 칼럼은 User Object의 속성으로 매핑되었습니다.

------

<span id='6'>

## Sequelize Sync 사용하기

Sequeliz에서는 **입력(INSERT), 수정(UPDATE), 조회(SELECT), 삭제(DELETE)**의 **`데이터 조작(DML: Data Manipulation Language)`**뿐만 아니라 데이터베이스의 **스키마 객체를 생성(CREATE), 변경(ALERT), 제거(DROP)** 할 수 있는 **`데이터 정의(DDL: Data Definition Language)`**도 지원합니다. 따라서 이미 만들어진 데이터베이스 테이블에 모델을 매핑할 수 있을 뿐만 아니라, **정의한 모델을 바탕으로 테이블을 생성할 수도 있습니다.(동기화)**

해당 기능을 사용하기 위해서는 Sequelize의 **sync** 메서드를 사용합니다. **`app.js`**에 다음의 코드를 추가합니다.

```
// connect To DB
const models = require('./models');
models.sequelize.sync()
  .then(() => {
    console.log('✓ DB connection success.');
    console.log('  Press CTRL-C to stop\n');
  })
  .catch(err => {
    console.error(err);
    console.log('✗ DB connection error. Please make sure DB is running.';
    process.exit();
  });
```

sync 메서드를 호출하여 실패했을 경우에는 에러 메시지를 출력 후 프로세스를 종료합니다.

sync 메서드는 모델에서 정의한 이름의 테이블이 존재하지 않을 경우에만 동작합니다. 이미 테이블이 존재할 경우에는 **models.sequelize.sync({force: true})** 과 같이 force 옵션을 주어 강제적으로 테이블을 제거 후 다시 생성이 가능하지만 매우 위험한 옵션이므로 주의를 기울여 사용해야 합니다.

------

<span id='7'>

## Sequelize 예제 (SELECT)

이제 Sequelize를 사용하여 SELECT를 사용해보겠습니다. 유저 리스트를 가져오는 query는 다음과 같습니다.

```
models.User.findAll()
  .then(results) {
     res.json(results);
  })
  .catch(err => {
     console.error(err);
  });
```

User 테이블에 있는 모든 row를 가져오는 query입니다. Sequelize는 결과를 Promise로 리턴하기 때문에 **findAll** 메서드 역시 Promise를 리턴합니다. 따라서 query의 결과는 then에서 받고, catch문에서 상황에 맞게 error 처리(handling)를 하면됩니다.

findAll의 더 자세한 사용법은 [Sequelize-model-findAll 설명](http://sequelize.readthedocs.io/en/latest/api/model/#findalloptions-promisearrayinstance)을 참고합니다.

------

<span id='8'>

## Sequelize 예제 (INSERT)

Sequelize를 사용하여 INSERT를 하는 방법은 다음과 같습니다.

```
models.User.create({userID: '유저ID', password: '유저PW'})
  .then(result => {
     res.json(result);
  })
  .catch(err => {
     console.error(err);
  });
```

**create** 메서드의 매개변수에 model에서 매핑한 내용을 토대로 데이터를 넣으면 query를 실행 후 insert된 row정보가 반환됩니다.

create의 더 자세한 사용법은 [Sequelize-model-create 설명](http://sequelize.readthedocs.io/en/latest/api/model/#createvalues-options-promiseinstance)을 참고합니다.

------

<span id='9'>

## Sequelize 예제 (UPDATE)

User 테이블의 데이터를 수정할때는 다음과 같이 사용합니다.

```
models.User.update({password: '새로운 유저PW'}, {where: {userID: '유저ID'}})
  .then(result => {
     res.json(result);
  })
  .catch(err => {
     console.error(err);
  });
```

**update** 메서드의 매개변수에는 update할 데이터를 입력합니다.

update 더 자세한 사용법은 [Sequelize-model-update 설명](http://sequelize.readthedocs.io/en/latest/api/model/#updatevalues-options-promisearrayaffectedcount-affectedrows)을 참고합니다.

------

<span id='10'>

## Sequelize 예제 (DELETE)

User 테이블의 데이터를 삭제할때는 다음과 같이 사용합니다.

```
models.User.destroy({where: {userID: '유저ID'}})
  .then(result => {
     res.json({});
  })
  .catch(err => {
     console.error(err);
  });
```

**destroy** 메서드의 매개변수에는 where 조건을 입력합니다.**(where 조건을 입력하지 않을 경우 테이블의 모든 row가 삭제되기 때문에 주의해야 합니다.)**

Reference:

보통 **Node.js 를 실행**할 때  **NODE_ENV 값을 이용**해서     **production ( 배포 ) 모드**  **development ( 개발 ) 모드**   이렇게 두 가지로 나누어서 실행하게 되는데요.   Node.js 프레임워크인 [**Express**](http://expressjs.com/) 의 경우   **production 모드** 일때는  **파일 캐싱, 에러 메시지 감추기** 등 배포의 적합한 환경 설정을 하구요.  **development 모드** 일 때는  **파일 캐싱 방지, 디버그를 위한 상세한 에러 메시지 보이기** 등   개발에 도움을 줄 수 있는 환경으로 설정을 해줍니다.   하지만 직접 이 값을 알아 내고 싶을 때가 있는데요.  이럴 때는 Express 에서 전체 앱을 총괄하는 역할을 맡고 있는 **root 디렉토리의 app.js** 에서 **첫 줄**에  ` process.env.NODE_ENV = ( process.env.NODE_ENV && ( process.env.NODE_ENV ).trim().toLowerCase() == 'production' ) ? 'production' : 'development'; ` 이렇게  해주면 NODE_ENV 가 따로 지정이 안되어 있더라도 development 모드로 지정되게 해줄 수 있습니다.  이후 **이 앱 소스 어디에서나 ( require 된 파일 포함 )** `if( process.env.NODE_ENV == 'production' ) { console.log("Production Mode"); } else if( process.env.NODE_ENV == 'development' ) { console.log("Development Mode"); }` 위 처럼 NODE_ENV 값에 따라 하고 싶은 일을 함으로써 개발과 배포용 소스를 따로 분리해서 코딩 할 수 있습니다.  출처: [http://inspiredjw.com/entry/Nodejs-에서-NODEENV-값으로-환경-설정하기](http://inspiredjw.com/entry/Nodejs-%EC%97%90%EC%84%9C-NODEENV-%EA%B0%92%EC%9C%BC%EB%A1%9C-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0) [Inspired World]







