# SpringBoot MultiTenancy [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 
## Features
* _Auto creates Database/Schema_ per tenant (not tested for external DataSource)
* _Auto creates tables_

This library built for Multi-Tenancy support using **SpringBoot v2.5.2**.

Class | Type | args | Description
--- | --- | --- | ---
Database_Based_MultiTenancy | Annotation | basePackages | Activates **Database** per Tenant feature 
Schema_Based_MultiTenancy | Annotation | basePackages | Activates **Schema** per Tenant feature 
TenantHolder | Annotation | - | Defines Entity to use as Database Configuration holder
DataSourceComponent | Class | - | Extend to Entity annotated with TenantHolder, Holds specific properties of Database
DataSourceComponent | Class | - | Extend to Entity annotated with TenantHolder, Holds specific properties of Database


## Usage
Add dependency into pom.xml to import library.

```
        <dependency>
            <groupId>io.gitlab.rujal_sh</groupId>
            <artifactId>springBoot-multiTenant</artifactId>
            <version>1.0.4</version>
        </dependency>
```

Multi-Tenancy action can be activated in two ways. 

##### 1. Database per Tenant: Each Tenant will have its own database.
###### Firstly, Add _@Database_Based_MultiTenancy_ annotation to Configuration class

Example: 
```
@SpringBootApplication
@Database_Based_MultiTenancy(basePackages = "com.foo")
public class MultiTenantApplication {

    public static void main(String[] args) {
       ...
    }

}
```
**@Database_Based_MultiTenancy** annotation activates only Database per Tenant features, and **basePackages** indicates package to be scanned to create Entity OR Document.

###### Second, Add _@TenantHolder_ and extend class with _DataSourceComponent_ class to define Database configuration holder

Example:
```

@Entity
@TenantHolder
public class UserDetail extends DataSourceComponent {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    .
    .
    .
}
```
**@TenantHolder** specify configuration that this class is Tenant specifier whereas extending **DataSourceComponent** stores tenant specific database configuration with **@OneToOne** relation 

###### Finally, Run the project




##### 2. Schema per Tenant: Each Tenant will have its own database.
###### Firstly, Add _@Schema_Based_MultiTenancy_ annotation to Configuration class

Example: 
```
@SpringBootApplication
@Schema_Based_MultiTenancy(basePackages = "com.foo")
public class MultiTenantApplication {

    public static void main(String[] args) {
       ...
    }

}
```

###### Second, Add _@TenantHolder_ to specify tenant holder. Here, _DataSourceComponent_ has no use

Example:
```

@Entity
@TenantHolder
public class UserDetail {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    .
    .
    .
}
```

**Note:**  The TenantId value should be passed by Tenants and should match the **database** OR **schema** name to support corresponding functionality.

##Tutorials
Database MultiTenancy => https://github.com/ShresthaRujal/Tutorials/tree/master/multitenant
