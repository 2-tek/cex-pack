# SCHEMA

## 1. Entities

```sql
CREATE TABLE test_suite (
    id            BIGINT        PRIMARY KEY,
    name          VARCHAR(255)  NOT NULL,
    version       VARCHAR(64)   NOT NULL,
    quick_version VARCHAR(32)   NOT NULL,
    target_path   VARCHAR(1024) NOT NULL,
    created_at    TIMESTAMP     NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE test_compiler (
    id            BIGINT        PRIMARY KEY,
    name          VARCHAR(255)  NOT NULL,
    version       VARCHAR(64)   NOT NULL,
    binary_path   VARCHAR(1024) NOT NULL,
    created_at    TIMESTAMP     NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE test_runner (
    id            BIGINT        PRIMARY KEY,
    name          VARCHAR(255)  NOT NULL,
    edition       VARCHAR(64)   NOT NULL,
    binary_path   VARCHAR(1024) NOT NULL,
    created_at    TIMESTAMP     NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## 2. Relationships

```text
test_suite -1:1-1:1- test_runner
test_suite -1:1-1:1- test_compiler
```

## 3. API (/api)

| Method | Path               | Entity        | Action            |
| :----- | :----------------- | :------------ | :---------------- |
| GET    | /api/suites        | test_suite    | list test suites  |
| GET    | /api/suites/:id    | test_suite    | get test suite    |
| GET    | /api/compilers     | test_compiler | list compilers    |
| GET    | /api/compilers/:id | test_compiler | get compiler info |
| GET    | /api/runners       | test_runner   | list test runners |
| GET    | /api/runners/:id   | test_runner   | get runner info   |
