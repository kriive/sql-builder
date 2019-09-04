# sql-builder

[![Build Status](https://travis-ci.org/six-ddc/sql-builder.svg?branch=master)](https://travis-ci.org/six-ddc/sql-builder)

♥️ SQL query string builder for C++11

## Examples:

``` c++
    using namespace sql;

    InsertModel i;
    i.insert("score", 100)
            ("name", std::string("six"))
            ("age", (unsigned char)20)
            ("address", "beijing")
            ("create_time", nullptr)
        .into("user");
    assert(i.str() ==
            "insert into user(score, name, age, address, create_time) values(100, 'six', 20, 'beijing', null)");

    SelectModel s;
    s.select("id", "age", "name", "address")
        .from("user")
        .where(column("score") > 60 and (column("age") >= 20 or column("address").is_not_null()))
        // .where(column("score") > 60 && (column("age") >= 20 || column("address").is_not_null()))
        .group_by("age")
        .having(column("age") > 10)
        .order_by("age desc")
        .limit(10)
        .offset(1);
    assert(s.str() ==
            "select id, age, name, address from user where (score > 60) and ((age >= 20) or (address is not null)) group by age having age > 10 order by age desc limit 10 offset 1");

    std::vector<int> a = {1, 2, 3};
    UpdateModel u;
    u.update("user")
        .set("name", "ddc")
            ("age", 18)
            ("score", nullptr)
            ("address", "beijing")
        .where(column("id").in(a));
    assert(u.str() ==
            "update user set name = 'ddc', age = 18, score = null, address = 'beijing' where id in (1, 2, 3)");

    DeleteModel d;
    d._delete()
        .from("user")
        .where(column("id") == 1);
    assert(d.str() ==
            "delete from user where id = 1");
```
