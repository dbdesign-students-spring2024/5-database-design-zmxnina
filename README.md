# Data Normalization and Entity-Relationship Diagramming
### I would like to use my second automatic extension for this assignment. Thank you!

## Original Dataset
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading   | professor_email  |
|---------------|------------|----------|-----------|---------------------------------|-----------|-------|-------------------|------------------|
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3 | l.melvin@foo.edu |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11| e.logston@foo.edu|
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3 | l.melvin@foo.edu |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14| e.logston@foo.edu|
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87    | i.nevarez@foo.edu|
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                | ...              |

## Noncompliance with 4NF
In order for the dataset to be compliant with 4NF, it must first satisfy the requirements of the previous normal forms (i.e. 1NF, 2NF, and 3NF) and additionally, the dataset should not contain more than one independent multi-valued fact about an entity. Given that the dataset is already in 1NF, we examine the succeeding forms.

#### Second Normal Form (2NF)
Since the dataset stores students' grades in courses at a university, the combination of "assignment_id" and "student_id" is likely used as a composite primary key, ensuring that each record for a specific assignment by a specific student is unique. 
    
The dataset must be considered for 2NF since it involves the use of a composite primary key. 2NF requires all non-key fields to be fully dependent on the entire composite key. In this case, the non-key fields "due_date", "assignment_topic", "relevant_reading" are dependent on the assignment alone and does not contain any information on the student. Hence, the dataset does not comply with 2NF.

#### Third Normal Form (3NF)
Practically speaking, the dataset cannot be considered for 3NF since it has already violated 2NF. However, even if the dataset did meet 2NF, it would still violate the additional requirements for 3NF. We observe that the "professor_email" is fully dependent on the "professor." In this case, 3NF is violated since a non-key field contains information about another non-key field.

#### Fourth Normal Form (4NF)
    
Again, since the dataset does not meet the requirements of the previous normal forms, it cannot be considered for 4NF. However, even if the dataset did meet the previous forms, it will still violate the additional requirements for 4NF.

First, there are multiple entities in the given dataset (either explicitly or implicitly implied): students, professors, courses, sections, classrooms, assignments, due dates, and grades. To acheive a normalized database design, these distinct entities should all have its own table to minimize redundancy and dependency. 

Further, under 4NF, the dataset should not contain more than one independent multi-valued fact about an entity. However, the following cases of multi-valued dependencies can be deduced from the dataset:

- For a given course, there can be multiple sections taught by the same or different professors in the same or different classrooms. Since any professor can teach a course in any classroom, there is no direct connection between the "professor" and the "classroom;" hence, they are two independent multi-valued facts tied to the same entity (courses).

- A professor can designate the same or different assignment topic to multiple sections of the same course, each with the same or different due dates. Since any assignment topic can be associated with any due date, contingent on the professor and the section of the course being taught, there is no direct connection between the "assignment_topic" and the "due_date;" hence, they are two independent multi-valued facts tied to the same entity (professors).
    
    Hence, the dataset does not comply with 4NF.

## 4NF Compliant Dataset
To resolve the suggested violations, we decompose the original dataset into the following tables:

1. **Students**

    | student_id** | first_name | last_name | email               |
    |--------------|------------|-----------|---------------------|
    | 1            | Robert     | Erickson  | r.erickson@foo.edu  |
    | 2            | Peter      | Myers     | p.myers@foo.edu     |
    | 3            | Jessica    | Doyle     | j.doyle@foo.edu     |
    | 4            | John       | Bell      | j.bell@foo.edu      |
    | 5            | Tanya      | Robinson  | t.robinson@foo.edu  |
    | ...          | ...        | ...       | ...                 |

2. **Professors**

    | professor_id** | first_name | last_name | email               |
    |----------------|------------|-----------|---------------------|
    | 1              | Laura      | Melvin    | l.melvin@foo.edu    |
    | 2              | Edward     | Logston   | e.logston@foo.edu   |
    | 3              | Isabel     | Nevarez   | i.nevarez@foo.edu   |
    | 4              | Mary       | Miller    | m.miller@foo.edu    |
    | 5              | Pamela     | Gray      | p.gray@foo.edu      |
    | ...            | ...        | ...       | ...                 |

3. **Courses**

    | course_id** | name                                 | credits |
    |-------------|--------------------------------------|---------|
    | 1           | Introduction to Computer Science     | 4       |
    | 2           | Principles of Economics              | 3       |
    | 3           | Database Design and Implementation   | 4       |
    | 4           | General Chemistry                    | 3       |
    | 5           | World History                        | 2       |
    | ...         | ...                                  | ...     |

4. **Classrooms**

    | classroom_id** | location_code | room_number |
    |----------------|---------------|-------------|
    | 1              | WWH           | 101         |
    | 2              | 60FA          | 314         |
    | 3              | WWH           | 201         |
    | 4              | 10WP          | 920         |
    | 5              | 15MTC         | 711         |
    | ...            | ...           | ...         |

5. **Sections**

    | section_id** | course_id | professor_id | classroom_id |
    |--------------|-----------|--------------|--------------|
    | 1            | 7         | 1            | 4            |
    | 2            | 7         | 2            | 2            |
    | 3            | 7         | 1            | 5            |
    | 4            | 3         | 3            | 12           |
    | 5            | 3         | 5            | 2            |
    | ...          | ...       | ...          | ...          |

6. **Assignments**

    | assignment_id** | topic                             | relevant_reading     |
    |-----------------|-----------------------------------|----------------------|
    | 1               | Data normalization                | Deumlich Chapter 3   |
    | 2               | Single table queries              | Dümmlers Chapter 11  |
    | 3               | Advanced Database Systems         | Smithson Textbook 22 |
    | 4               | Spreadsheet aggregate functions   | Zehnder Page 87      |
    | 5               | Python and pandas                 | Dümmlers Chapter 14  |
    | ...             | ...                               | ...                  |

7. **Due Dates**

    | assignment_id** | section_id** | due_date  |
    |-----------------|--------------|-----------|
    | 1               | 5            | 23.02.21  |
    | 2               | 3            | 18.11.21  |
    | 1               | 1            | 23.02.21  |
    | 5               | 3            | 05.05.21  |
    | 4               | 4            | 04.07.21  |
    | ...             | ...          | ...       |

8. **Grades**

    | student_id** | assignment_id** | grade |
    |--------------|-----------------|-------|
    | 1            | 1               | 80    |
    | 7            | 2               | 25    |
    | 4            | 1               | 75    |
    | 2            | 5               | 92    |
    | 2            | 4               | 65    |
    | ...          | ...             | ...   |

Note: primary keys are indicated wih **
## ER Diagram
![ER diagram for 4NF compliant dataset](images/er_diagram.svg)

## Nomalization Explanation
The following changes were made to the original dataset to achieve a 4NF compliant database design:

#### Decomposition into Multiple Tables
As shown above, the dataset was divided into 8 distinct tables, each focusing on a specific entity: students, professors, courses, classrooms, sections, assignments, due dates, and grades. By creating separate tables for related entities, we eliminate the presence of multi-valued dependencies (MVDs), where more than one independent multi-valued fact about an entity existed within the same table. 

Specifically, to resolve the MVD between courses, professors, and classrooms, separate tables were created for each entity and a new sections table was introduced to reflect their unique interrelations through foreign keys. 

Likewise, to resolve the MVD between professors, assignment topics, and due dates, a separate assignments table was created to avoid excessive redundancies in repeating assignment topics and relevant readings. The unique interrelations between assignments, sections, and due dates are refelected in a newly constructed due dates table through the use of foreign keys.

#### Introduction of Primary Key Fields
Each table is designed to include appropriate primary keys to uniquely identify each of the included records. Specifically, the due dates table make use of composite keys to reflect unique combinations of assignments and sections; the grades table make use of composite keys to reflect unique combinations of students and assignments. Further, each table includes only non-key attributes that are fully functional on the primary key(s). This eliminates the presence of transitive dependencies and hence, any violations to 2NF and 3NF.

#### Inclusion of Missing Non-key Fields
Non-key fields were added to some of the tables to provide additional information that deemed necessary to the educational domain. Specifically, the students table included "first_name" and "last_name", and "email" to provide basic information about each of the enrolled students. The courses table included "name" and "credit" to provide basic information about each of the offered courses. The classroom information in the original dataset is separated into two fields in the classrooms table - "location_code" and "room_number". This allows for the potential of a locations table (with "location_code" as the primary key) that stores detailed addresses and additional information on campus buildings.
