---
title: spring-boot-application-context
date: 2020-07-18 23:56:20
tags: SpringBoot
---

### Annotation Config Servlet Application Context (Spring boot 2.3.1 Version 기준)
![](/images/springboot/applicationContext/annotation_config_servlet_application_context_class_diagram.png)

- AnnotationBeanDefinitionReader
![](/images/springboot/applicationContext/annotated_bean_definition_reader.png)
    - Field 설명
        - BeanNameGenerator : 
    - 기능 설명
    - 명칭은 Reader인데 Bean을 등록하는 함수를 가지고 있다.
    - registerBean과 Environment 생성을 담당한다.
        - Environment : Java System의 Environment로 System Properties(String value)와 System Environment(Key/Value)를 propertySource에 등록한다.
        - registerBean :  
- ClassPathBeanDefinitionScanner