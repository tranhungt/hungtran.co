---
title: "Mapping Sql Entities With Java Hibernatejpa"
date: 2016-03-12T19:04:14-05:00
draft: false
---

In learning and working with Java Hibernate/JPA in mapping sql entities, I found it initially confusing as to how some data types can be mapped while others requiring special workarounds. For starters, SQL array types are considered custom, and not supported directly. Java 8 java.time LocalDate and LocalDateTime are not supported. And mapping One-To-Many relationships can be a little tricky, so I'll lay out some of the examples here as a nifty reference.

### Mapping SQL Arrays to Java List
It's strange that SQL databases like Postgres have implemented array field types for some time, JPA still does not deem it necessary to support it out of the box. In order to map SQL arrays, we will need to implement a custom UserType. A UserType is a way for JPA to allow custom interpretation of any sql data. So here we will show how a sql array field type can be mapped to a Java List. We will assume that the sql field is an array of string types.

First we implement the UserType interface. By contract, we must implement a series of methods, two of which are the most important, one to parse the data from the sql field (nullSafeGet), and one to serialize and save into sql(nullSafeSet).

```
public class CustomStringList implements UserType {
    @Override
    public int[] sqlTypes() {
        return new int[] {Types.VARCHAR};
    }

    @Override
    public Class returnedClass() {
        return List.class;
    }

    @Override
    public boolean equals(Object o, Object o1) throws HibernateException {
        return ObjectUtils.equals(o, o1);
    }

    @Override
    public int hashCode(Object o) throws HibernateException {
        if (o != null)
            return o.hashCode();
        else
            return 0;
    }

    @Override
    public Object nullSafeGet(ResultSet resultSet, String[] strings, SessionImplementor sessionImplementor, Object o) throws HibernateException, SQLException {
        List<String> list = null;
        String nameVal = resultSet.getString(strings[0]);
        if (nameVal != null) {
            nameVal = nameVal.substring(1,nameVal.length()-1);
            list = new ArrayList<>();
            StringTokenizer tokenizer = new StringTokenizer(nameVal, ",");
            while (tokenizer.hasMoreElements()) {
                String val = (String) tokenizer.nextElement();
                list.add(val);
            }
        }
        return list;
    }

    @Override
    public void nullSafeSet(PreparedStatement preparedStatement, Object o, int i, SessionImplementor sessionImplementor) throws HibernateException, SQLException {
        if (o == null) {
            preparedStatement.setNull(i, Types.VARCHAR);
        } else {
            preparedStatement.setString(i, serialize((List<String>) o));
        }
    }

    @Override
    public Object deepCopy(Object o) throws HibernateException {
        return o;
    }

    @Override
    public boolean isMutable() {
        return false;
    }

    @Override
    public Serializable disassemble(Object o) throws HibernateException {
        return (Serializable) o;
    }

    @Override
    public Object assemble(Serializable serializable, Object o) throws HibernateException {
        return serializable;
    }

    @Override
    public Object replace(Object o, Object o1, Object o2) throws HibernateException {
        return o;
    }

    private String serialize(List<String> list) {
        StringBuilder strbul = new StringBuilder();
        Iterator<String> iter = list.iterator();
        strbul.append("{");
        while (iter.hasNext()) {
            strbul.append(iter.next());
            if (iter.hasNext()) {
                strbul.append(",");
            }
        }
        strbul.append("}");
        return strbul.toString();
    }
}
```

This custom UserType we just made, CustomStringList is now ready to be used. It can be referenced via the `@Type` annotation.
```
@Entity
@Table(name = "user")
public class User {

    @Type(type="my.project.entities.util.CustomStringList")
    @Column(name = "items")
    private ArrayList<String> items;
}
```

### Using Java 8's java.time Date and DateTime Types
The latest JPA spec, Version 2.1, came out before Java 8, therefore there are no mandates to support the new Java 8 APIs such as java.time classes. Hibernate therefore only supports java.util Date and DateTime when mapping SQL Date types, while those wanting to use the newer LocalDateTime and LocalDate will have to use workarounds when mapping to SQL date fields.

For example, a User class with a 'join_date' field.
```
@Entity
@Table(name = "user")
public class User {

    @Column(name = "join_date")
    private LocalDate joinDate;
}
```

In order to use the LocalDate type, one can implement the AttributeConverter in order to automatically map sql date types to use the new java.time libraries. By setting `autoApply = true`, the mapping will happen automatically for all `LocalDate` type fields.

```
@Converter(autoApply = true)
public class LocalDateAttributeConverter implements AttributeConverter<LocalDate, Date> {
    @Override
    public Date convertToDatabaseColumn(LocalDate locDate) {
        return (locDate == null ? null : Date.from(locDate.atStartOfDay().atZone(ZoneId.systemDefault()).toInstant()));
    }

    @Override
    public LocalDate convertToEntityAttribute(Date sqlDate) {
        return (sqlDate == null ? null : sqlDate.toInstant().atZone(ZoneId.systemDefault()).toLocalDate());
    }
}

```

### Mapping One-To-Many Relationships
In this example, we'll model a user having multiple roles, mapped via a one-to-many relationships.
When the Role table has a foreign key column, such as `user_id`. In this case, setting the `@JoinColumn` `name` parameter in a `@OneToMany` will automatically join Book's user_id column to the User's primary key `id` column. However, if you want to specify a join column on User other than the `id` column, you can specify the column in the `referencedColumnName ` parameter in the `@JoinColumn` annotation. One must be careful though, that `name` and `referencedColumnName` references different things depending on the mapping relationship being used.

```
@Entity
@Table(name = "user")
public class User {

    @Id
    @GeneratedValue
    @Column
    private Long id;
    
    @OneToMany(fetch = FetchType.EAGER)
    @JoinColumn(name = "user_id")
    private List<Book> books;
}

@Entity
@Table(name = "role")
public class Role {

    @Column(name = "user_id")
    private Long userId;

    @Column
    private String title;
}
```


