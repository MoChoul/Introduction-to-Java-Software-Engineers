@Autowired 默认是按照类型注入的，如果这个类型没找到，会根据名称去注入，如果在用的时候需要指定名称，可以加注解@Qualifier("指定名称的类")

@Resource注解也可以从容器中注入bean，默认是按照名称注入的，如果这个名称的没找到，就会按照类型去找，也可以在注解里直接指定名称@Resource(name="类的名称")