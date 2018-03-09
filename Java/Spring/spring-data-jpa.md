## Spring JPA

*Jpa 依赖于 EntityManagerFactory 和 TransactionManager*

### Jpa 注解命名规则

> @Named* 注解与实体类，声明一个策略，与 Jpa repository 中@* 或 规范的方法命名 结合使用

> eg: @NamedQuery(name = "User.findByEmailAddress", query = "select u from User u where u.emailAddress = ?1") 与 repository 中的 findByEmailAddress 方法

> @NamedEntityGraph(name = "GroupInfo.detail", attributeNodes = @NamedAttributeNode("members")) 与 @EntityGraph(value = "GroupInfo.detail", type = EntityGraphType.LOAD)

### Jpa 执行策略优先级顺序

> 1. @Query注解

> 2. 注解与实体类的@NamedQuery注解

> 3. 符合规范的方法命名

### Jpa projection 映射

> 1. 使用接口将实体类的属性映射为特定接口的方法(可以递归)

>> `interface PersonSummary {
  String getFirstname();
  String getLastname();
  AddressSummary getAddress();
  interface AddressSummary {
  String getCity();
  }
}
interface NamesOnly {
  @Value("#{target.firstname + ' ' + target.lastname}")
  String getFullName();
  …
}
interface NamesOnly {
  String getFirstname();
  String getLastname();
  default String getFullName() {
  return getFirstname.concat(" ").concat(getLastname());
  }
}`

> 2. 使用类将实体类的属性映射为特定类属性

>> 可以结合 DTO 使用

### Spring transaction

> 尽量使用 @Transactional 的 readOnly属性，当使用 hibernate 的 entityManager 时会进行优化(避免刷新脏数据操作)

### 使用 Auditing 审计数据更新

> 1. 用@CreatedBy @LastModifiedBy @CreatedDate @LastModifiedDate 注解实体字段，记录更新信息.

> 2. 注册类型为 AuditorAware 的bean实现 getCurrentAuditor 方法，以获取当前用户.

> 3. 在实体类上注解 @EntityListeners(AuditingEntityListener.class) 实现更新时审计.


