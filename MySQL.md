### Null 대체

- `IFNULL(column_name, 대체값)`

### View

- 만들기
  - `CREATE VIEW _____ AS <query expression>`
- 지우기
  - `DROP VIEW _____`

### Procedure

- 만들기

```mysql
DELIMITER //
CREATE PROCEDURE procedure_name(argument)
BEGIN
	내용
END
//
DELIMITER ;
```

- 지우기
  - `DROP PROCEDURE procedure_name`

