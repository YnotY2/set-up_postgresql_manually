# Visualising Database Schema:

When creating a new database for any projects, the schema a.k.a the layout of the database will always differ! This is logical ofcours as different data needs to be saved wihin different layouts. First off it's important to note what a database 
actually contains and the official terms corresponding. 

## Every DB has the following; 

+ name-datbase 1x
+ name-schema 1-20x
+ name-table 1-20x
    + name-id             (within the table)
    + name-data           (within the table)
    + created_at          (within the table)


Here is a example database witch has all the standard listed above things:
```bash

saving_mail_templates                                                                       (NAME DATABASE)
│
├── mail_content_template                                                                  (NAME SCHEMA) 
│   └── template_data                                                                      (NAME TABLE)
│       ├── template_id (SERIAL)                                                           (NAME ID)
│       ├── template_data (JSONB)                                                          (NAME DATA)
│       └── created_at (TIMESTAMP)                                                         (NAME TIME-CREATED)
│                                                                                           

```


## Terms every DB has explained: 

1, "database"
This one I think is very self explanetory, you can simply specify a name when creating a dabatase. 

2, "schema" 
You can view the schema within a database as the second directory within a database. It is helpfull to me to view a entire db as a directory tree. (see above visualisation) 
Within a schema tables reside, to the schema it's self is more like a indicator which tables reside there. 

So a schema is simply a directory that makes the whole structure of a db more clear and logical. Like "/passwords/" would likely contain passwords within that dir... "/passwords/mail_pass.txt" 

3, "tables" 
As listed above in the visual a table always resides within a schema. The table within a schema (within a db) acts as the last directory before the actuall relevant information.  Within the table we can find the actuall JSONB object, 
the creation time. The time the object was added to the table. And finally the ID of the entry. 

Simply the table carries the information we actually want to fetch from a database. If thinking of it as a dir, it would look like so ""/passwords/mail/yahoo.txt" The relevant information clearly structured :D 


4, 5, 6 
The last 3 terms are very much self explanetory, so I am not even going to explain them. Read ther'e names... 
name-id             (within the table)
name-data           (within the table)
created_at          (within the table)








