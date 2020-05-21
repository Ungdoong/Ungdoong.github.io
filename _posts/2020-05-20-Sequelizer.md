# Sequelizer



Sequlize.js는 Node.js 기반의 ORM(Object-Relational-Mapping)

- MySQL, MariaDB, SQLite, MS-SQL을 지원

## 설치

_______

```shell
$ npm i sequelize
```

- node-mysql도 설치하여야 함

  ```shell
  $ npm i mysql
  ```

## 연동

______

```javascript
const Sequelize = require('sequelize');

const sequelize = new Sequelize(
  'practice', // 데이터베이스 이름
  'username', // 유저 명
  'password', // 비밀번호
  {
    'host': 'localhost', // 데이터베이스 호스트
    'dialect': 'mysql' // 사용할 데이터베이스 종류
  }
);
```

사용하려는 DBMS가 당연히 설치되어 있어야 하고 해당 DB와 유저 등이 존재하고 있어야 함

## 모델 정의

________

```javascript
sequelize.define('Memo', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  title: {
    type: Sequelize.STRING,
    allowNull: true
  },
  body: {
    type: Sequelize.TEXT,
    allowNull: true
  }
});
```

- Sequelize는 모델을 정의하면 `createAt`, `updateAt` 필드를 자동으로 만듬
- 테이블에 레코드를 생성하거나 업데이트할 때 자동으로 관리

**Table: Memo**

| Field | Type | Allow Null | Key |
|--------|---------|-------------|-------|
| id | INT | false | PRI |
| title | VARCHAR(255) | true | - |
| body | TEXT | true | - |
| createdAt | DATETIME | false | - |
| updatedAt | DATETIME | false | - |



## 관계 정의

_______

관계 정의를 위해 모델을 하나 더 정의

```javascript
const Label = sequelize.define('Label', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: Sequelize.STRING,
    allowNull: false,
    unique: true
  }
});
```

Label과 Memo 사이에 N:M관계를 정의하기 위해 `belongsToMany()`라는 메소드를 사용

```javascript
Memo.belongsToMany(Label, { through: 'MemoLabel' });
Label.belongsToMany(Memo, { through: 'MemoLabel' });
```

관계 정의를 하면 교차테이블이 생성됨

**Table: MemoLabel**

| Field | Type | Allow Null | Key |
|-------|------|-----|-----|
| LabelId | INTEGER | false | PRI |
| MemoId | INT | false | MUL |
| createdAt | DATETIME | false | - |
| updatedAt | DATETIME | false | - |

**Relation for table: MemoLabel**
| Name | Columns | FK Table | FK Columns | On Update | On Delete |
|---|---|---|---|---|
| memolabel_ibfk_1 | LabelId | Labels | id | CASCADE | CASCADE |
| memolabel_ibfk_2 | MemoId | Memos | id | CASCADE | CASCADE |

- 1:1 관계 - `hasOne()`, `belongsTo()`
- 1:M 관계 - `hasMany()`

**※ `DELETE`시에 `CASCADE`되는 것은 N:M관계일 때 뿐이며, 1:1, 1:M 관계에서는 `SET NULL`**



## CRUD

_______

### Create - Create()

```javascript
Memo.create({
  title: 'Practice of Sequelize.js',
  body: 'Sequelize.js is ORM for Node.js.'
});
```

- 관계가 있는 모델의 교차테이블에 레코드 생성

  ```javascript
  memo.addLabel(label);
  ```

  - `addLabel()`은 관계를 정의하면 테이블명을 이용해서 자동생성되는 메소드
  - 배열을 추가하는 메소드는 `addLabels()`
  - 이 외에 `getLabels()`, `setLabels()` 등이 자동 생성됨
  - Label 모델에도 동일하게 `addMemo()`와 같은 메소드가 생성됨

### Read - findOne(), findAll()

```javascript
Memo.findOne({
  where: { title: 'Practice of Sequelize.js' }
})
.then((memo) => {
  console.log('Memo: ', memo.dataValues);
});
```

- 레코드의 실제 값은 `dataValues`라는 프로퍼티 안에 존재

- 조회 시 사용할 수 있는 여러가지 [오퍼레이터](#https://sequelize.org/master/manual/querying.html#operators)가 존재

- 교차테이블의 레코드 조회

  ```javascript
  memo.getLabels()
  .then((labels) => {
    console.log('Memo\'s Label: ', labels[0].dataValues);
  });
  ```

- `include`라는 프로퍼티를 사용하면 조인된 상태의 데이터를 조회 가능

  ```javascript
  Memo.findOne({
    where: { title: 'Practice of Sequelize.js' },
    include: { model: Label }
  })
  .then((memo) => {
    console.log('Joined Memo: ', memo.dataValues);
  });
  ```

- `findOrCreate()` - 조회 후에 없는 값이면 생성, 있는 값이면 값을 가져옴

  ```javascript
  Memo.findOrCreate({
    where: { title: 'Mongoose.js' },
    defaults: {
      body: 'Mongoose.js is ODM for Node.js.'
    }
  })
  .spread((memo, created) => {
    if (created) {
      console.log('New Memo: ', memo.dataValues);
    } else {
      console.log('Old Memo: ', memo.dataValues);
    }
  });
  ```

  - 반환 값이 두가지 이상이므로 `then()`은 사용불가 → `spread()`를 사용



### Update - update()

```javascript
Memo.update({
  title: 'Updated Memo'
}, {
  where: { id: 1 }
})
.then(() => {
  return Memo.findOne({
    where: { title: 'Updated Memo' }
  });
})
.then((memo) => {
  console.log('Updated Memo: ', memo.dataValues);
});
```

- 레코드 자체의 업데이트 메소드를 활용할 수 있음

  ```javascript
  memo.update({ body: 'This memo is updated.' })
  .then((memo) => {
    console.log('Updated Memo2: ', memo.dataValues);
  });
  ```

  - But, Primary Key를 업데이트 하는 것은 불가능

### Delete - destroy()

```javascript
Memo.destroy({
  where: { title: 'Updated Memo' }
})
.then(() => {
  return Memo.findOne({ where: { title: 'Updated Memo' } });
})
.then((memo) => {
  console.log('Destroyed Memo? :', memo); // null
});
```

- 레코드의 업데이트 메소드를 사용할 수도 있음

  ```javascript
  Memo.findById(2)
  .then((memo) => {
    return memo.destroy();
  })
  .then(() => {
    console.log('Memo is destroyed.');
  });
  ```

  