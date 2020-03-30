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

    /* meta-syntax */
    
    interface Converter<S, T> {

       T convert(S source);   
        
    }
    
 and
    
    interface ConversionService {
        
        T convert(S source, Class<T> targetType);
	
    }
    
The convervion service is a registry of converters and it delegates to its registered converters to carry out type conversion logic.
Developer has to call conversion service for any needed conversion

    /* meta-syntax */
    TargetType targetObject = conversionService.convert(sourceObj, TargetType)


**j-metamorphosis** follows the model of type conversions provided by Spring Framework. It offers an enabler (by annotations) for Spring Conversion Service and other utility classes to write lean converters.  
**metamorphosis-nestjs** brings to NestJS the missing concept of conversion service.

Benefits of a conversion service:
* Usually conversions are spread as static methods into many classes. With a conversion service, you have a class for each converter and you can put all converters in the same package. So, it will be easy to locate the conversion when you navigate your code.
* Usually conversions are implemented as static methods, but some conversions needs to query a repository. With a conversion service, all converters could be singleton and you can inject repositories or other services into it.
* Usually to convert a type you have to convert all subtypes, creating a messy dependency chain. With a conversion service, according to SRP (single responsibility principle) a converter just does its part of conversion and it delegates to conversion service the remaining conversion of all subtypes. So, each converter has at most a dependency from the conversion service.

# Metamorphosis for Java

Import **j-metamorphosis** from maven repo

    <dependency>
      <groupId>it.fabioformosa</groupId>
      <artifactId>metamorphosis-core</artifactId>
      <version> ... </version>
    </dependency>
or
    <dependency>
      <groupId>it.fabioformosa</groupId>
      <artifactId>metamorphosis-jpa</artifactId>
      <version> ... </version>
    </dependency>
   
add `@EnableMetamorphosisConversions` to your spring boot config class

    @Configuration
    @EnableMetamorphosisConversions(basePackages = { "your.package" })
    public class MetamorphosisExampleConfig {

    }   
    
 To read how to create a converter and how to invoke the conversion service, go to [j-metamorphosis README](https://github.com/fabioformosa/j-metamorphosis]).  

# Metamorphosis for Javascript

Import **metamorphosis-js** from npm:

``` npm install --save @fabio.formosa/metamorphosis ```

Create a converter:

    import { Convert, Converter } from '@fabio.formosa/metamorphosis';

    @Convert(Car, CarDto)
    export default class CarToCarDtoConverter implements Converter<Car, CarDto> {
  
    public convert(source: Car): CarDto {
       const target = new CarDto();
       target.color = source.color;
       target.model = source.model;
       target.manufacturerName = source.manufacturer.name;
       return target;
    }

    }

The `Conversion Helper` is an alias for the conversion service:
 
    import { ConversionHelper } from '@fabio.formosa/metamorphosis'
    ...
    ConversionHelper conversionHelper = new ConversionHelper();
    const carDto = <CarDto> await conversionHelper.convert(car, CarDto);    

For further details about conversions, go to [metamorphosis-js README](https://github.com/fabioformosa/metamorphosis-js]).  

# Metamorphosis for NestJS

Import **metamorphosis-nestjs** from npm:

``` npm install --save @fabio.formosa/metamorphosis-nest ```

Import the `MetamorphosisModule` :

    import { MetamorphosisNestModule } from '@fabio.formosa/metamorphosis-nest';

    @Module({
      imports: [MetamorphosisModule.register()],
      ...
    }
    export class MyApp{
    }
    
Create a new converter class, implementing the interface Converter<Source, Target> and decorate the class with @Convert

    import { Convert, Converter } from '@fabio.formosa/metamorphosis';

    @Injectable()
    @Convert(Car, CarDto)
    export default class CarToCarDtoConverter implements Converter<Car, CarDto> {
  
      public convert(source: Car): CarDto {
         const target = new CarDto();
         target.color = source.color;
         target.model = source.model;
         target.manufacturerName = source.manufacturer.name;
         return target;
      }

    }

Use the conversion service:

    import { ConversionService } from '@fabio.formosa/metamorphosis-nest';

    @Injectable()
    class CarService{

      constructor(private convertionService: ConvertionService){}

      public async getCar(id: string): CarDto{
         const car: Car = await CarModel.findById(id);
         return <CarDto> await this.convertionService.convert(car, CarDto);
      }

    }
    
For further details about conversions, go to [metamorphosis-nestjs README](https://github.com/fabioformosa/metamorphosis-nestjs]).  

# Support

**j-metamorphosis** supports JPA in order to convert entities it DTOs and vice versa.  
**metamorphosis-nestjs** supports Mongoose/Typegoose and Type ORM.


 
