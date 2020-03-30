---
layout: default
---

> "Nothing is lost, nothing is created, everything is transformed"
> _Lavoisier
  
  
**Metamorphosis** is an utility library to ease conversions of objects.  
Most of development time is due to type conversions: from DTOs to entities, from entities to DTOs or from DTOs and other DTOs.  
Metamorphosis helps to write converters in a lean way and places them in a better structure within your project (further details in paragraphs below).

Metamorphosis supports several languages and frameworks:
* **[j-metamorphosis](https://github.com/fabioformosa/j-metamorphosis)** - Java with [Spring Boot](https://spring.io/projects/spring-boot)
* **[metamorphosis-js](https://github.com/fabioformosa/metamorphosis-js)** - (vanilla) Javascript/Typescript
* **[j-metamorphosis](https://github.com/fabioformosa/metamorphosis-nestjs)** - Typescript with [NestJS](https://nestjs.com/)  
  

![Chameleon - ph. Nandhu Kumar - pexels.com!](https://images.pexels.com/photos/312826/pexels-photo-312826.jpeg?auto=compress&cs=tinysrgb&h=325&w=470 "Chameleon - ph. Nandhu Kumar - pexels.com")

# Conversion Service

The conversion service is a component of a general type conversion system. The system defines an SPI to implement type conversion logic and an API to perform type conversions at runtime

    interface Converter<S, T> {

       T convert(S source);   
        
    }
    
 and
    
    interface ConversionService {
        
        T convert(S source, Class<T> targetType);
	
    }
    
The convervion service is a registry of converters and it delegates to its registered converters to carry out type conversion logic.      
**j-metamorphosis** follows the model of type conversions provided by Spring Framework. It offers an enabler (by annotations) for Spring Conversion Service and other utility classes to write lean converters.  
**metamorphosis-nestjs** brings to NestJS the missing concept of conversion service.

Benefits of a conversion service:
* Usually conversions are spread as static methods into many classes. With a conversion service, you have a class for each converter and you can put all converters in the same package. So, it will be easy to locate the conversion when you navigate your code.
* Usually conversions are implemented as static methods, but some conversions needs to query a repository. With a conversion service, all converters could be singleton and you can inject repositories or other services into it.
* Usually to convert a type you have to convert all subtypes, creating a messy dependency chain. With a conversion service, according to SRP (single responsibility principle) a converter just does its part of conversion and it delegates to conversion service the remaining conversion of all subtypes. So, each converter has at most a dependency from the conversion service.

[TBD] Activate it with j-metamorphosis and metamorphosis-nest

# Converters

**<< THIS PAGE IS WORK IN PROGRESS >>**
[TBD] HERE Example of some converters

# Supports

**<< THIS PAGE IS WORK IN PROGRESS >>**
[TBD] Jpa, Mongoose/Typegoose, TypeORM

 
