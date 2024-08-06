# Redis란?

Redis는 Remote Dictionary Server의 약자로 모든 데이터를 메모리에 저장하는 in-memory 데이터베이스이자, key-value 구조의 저장소이다.

# Redis의 구조

Redis는 다음과 같이 내부적인 구현이 되어 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/a172a5ce-2683-486f-aeff-d6575cad960c/e962e33a-1baf-45fc-be0d-38b25774f93d/Untitled.png)

Redis는 오픈소스이기 때문에 코드를 보면서 각각의 실제 구현을 확인할 수 있다.

## dictEntity

dictEntity는 개별 key-value 값을 저장하고 다음 key-value를 나타내는 포인터를 가지고 있다.

```jsx
// redis/deps/hiredis/dict.h

typedef struct dictEntry {
    void *key;
    void *val;
    struct dictEntry *next;
} dictEntry;
```

## dictType

dictType은 해시 테이블의 동작을 정의하는 함수를 정의하고, 다양한 데이터 구조를 지원한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/a172a5ce-2683-486f-aeff-d6575cad960c/f7703d73-c930-4e11-8745-7affb0281079/Untitled.png)

```jsx
// redis/deps/hiredis/dict.h

typedef struct dictType {
    unsigned int (*hashFunction)(const void *key); // 해시 값을 계산하는 함수
    void *(*keyDup)(void *privdata, const void *key);
    void *(*valDup)(void *privdata, const void *obj);
    int (*keyCompare)(void *privdata, const void *key1, const void *key2);
    void (*keyDestructor)(void *privdata, void *key);
    void (*valDestructor)(void *privdata, void *obj);
} dictType;
```

아래와 같이 다형적으로 변하며, 여러 데이터 구조를 지원한다.

```jsx
dictType setDictType = {
    dictSdsHash,               /* hash function */
    NULL,                      /* key dup */
    NULL,                      /* val dup */
    dictSdsKeyCompare,         /* key compare */
    dictSdsDestructor,         /* key destructor */
    NULL,                      /* val destructor */
    NULL,                      /* allow to expand */
    .no_value = 1,             /* no values in this dict */
    .keys_are_odd = 1          /* an SDS string is always an odd pointer */
};

dictType stringSetDictType = {
    dictCStrCaseHash,           /* hash function */
    NULL,                       /* key dup */
    NULL,                       /* val dup */
    dictCStrKeyCaseCompare,     /* key compare */
    dictSdsDestructor,          /* key destructor */
    NULL,                       /* val destructor */
    NULL                        /* allow to expand */
};
```

## dict

dict는 dictType과 dictEntity 포함하고 있는 해시 테이블이다.

dictType은 해시 함수와 데이터 구조를 정의하며,

dictEntity는 key-value를 저장한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/a172a5ce-2683-486f-aeff-d6575cad960c/9aaf8579-0c68-4415-866e-c673076db28b/Untitled.png)

```jsx
// redis/deps/hiredis/dict.h

typedef struct dict {
    dictEntry **table;
    dictType *type;
    unsigned long size;
    unsigned long sizemask;
    unsigned long used;
    void *privdata;
} dict;
```

## kvstore

kvsotre는 dict를 포함하고 있으며, 해시 테이블의 개수, 현재 리사이징 하고 있는지 여부, 총 키의 개수 등 해시테이블과 관련된 메타데이터를 관리한다.

```jsx
// redis/src/kvtore.c

struct _kvstore {
    int flags;
    dictType dtype;
    dict **dicts;
    long long num_dicts;
    long long num_dicts_bits;
    list *rehashing;                       /* List of dictionaries in this kvstore that are currently rehashing. */
    int resize_cursor;                     /* Cron job uses this cursor to gradually resize dictionaries (only used if num_dicts > 1). */
    int allocated_dicts;                   /* The number of allocated dicts. */
    int non_empty_dicts;                   /* The number of non-empty dicts. */
    unsigned long long key_count;          /* Total number of keys in this kvstore. */
    unsigned long long bucket_count;       /* Total number of buckets in this kvstore across dictionaries. */
    unsigned long long *dict_size_index;   /* Binary indexed tree (BIT) that describes cumulative key frequencies up until given dict-index. */
    size_t overhead_hashtable_lut;         /* The overhead of all dictionaries. */
    size_t overhead_hashtable_rehashing;   /* The overhead of dictionaries rehashing. */
};
```

## redisDb

redisDb는 kvsotre를 포함하고 있으며, 전체 데이터베이스를 관리한다.

```jsx
// redis/src/server.h

typedef struct redisDb {
    kvstore *keys;              /* The keyspace for this DB */
    kvstore *expires;           /* Timeout of keys with a timeout set */
    ebuckets hexpires;          /* Hash expiration DS. Single TTL per hash (of next min field to expire) */
    dict *blocking_keys;        /* Keys with clients waiting for data (BLPOP)*/
    dict *blocking_keys_unblock_on_nokey;   /* Keys with clients waiting for
                                             * data, and should be unblocked if key is deleted (XREADEDGROUP).
                                             * This is a subset of blocking_keys*/
    dict *ready_keys;           /* Blocked keys that received a PUSH */
    dict *watched_keys;         /* WATCHED keys for MULTI/EXEC CAS */
    int id;                     /* Database ID */
    long long avg_ttl;          /* Average TTL, just for stats */
    unsigned long expires_cursor; /* Cursor of the active expire cycle. */
    list *defrag_later;         /* List of key names to attempt to defrag one by one, gradually. */
} redisDb;
```

결론적으로 redis는 코드에서도 볼 수 있듯 메모리에 올라가는 key-value 형태의 해시 테이블(딕셔너리)로 구현이 되어 있으며 dictEntity를 이용해 다양한 형태의 자료구조를 지원한다.
