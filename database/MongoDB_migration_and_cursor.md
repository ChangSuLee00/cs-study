# MongoDB의 migration과 cursor

## 개요

MongoDB의 마이그레이션을 진행하면서 이미 배열인 aggregate 값을 toArray() 함수로 다시 배열 값으로 바꾸는 스니펫이 있어 그 이유를 찾아보았다.

```JavaScript
var ids = db.someSchema.aggregate([
  { $group: { _id: "$enterpriseId" } }
]).toArray().map(function(doc) { return doc._id; });
```

## Cursor

커서는 MongoDB에서 find 메서드를 실행하면 반환되는 문서의 집합으로, 기본적으로 자동으로 loop 되지만 명시적으로 특정 인덱스 문서를 가져올 수도 있다. Cursor는 특정 인덱스 값을 값는 포인터와 유사하다.

출처: https://www.softwaretestinghelp.com/mongodb/cursor-in-mongodb/

## How it works?

일반적으로 리턴값이 배열 형태로 보이기 때문에, Cursor가 쿼리 결과에 대한 포인터이며 그 자체가 배열이 아니라는 점에 대해 잘못 이해하기 쉽다. 정확하게 이야기 하자면 mongosh에서 실행된 find등의 명령어는 Document그 자체를 반환하지 않고 Cursor를 반환한다.

따라서 반환된 db.someSchema.aggregate([…])의 결과값은 Document의 배열이 아닌 Cursor 변수이고, 내장된 toArray()는 커서에서 반환된 모든 문서를 포함하는 배열을 반환하는 함수이다.

출처: https://www.mongodb.com/docs/manual/reference/method/cursor.toArray/#mongodb-method-cursor.toArray

## Document Size

참고로 일반적으로 Cursor는 처음부터 20개의 documents를 반환한다.

출처: https://www.mongodb.com/docs/manual/reference/method/cursor.batchSize/
