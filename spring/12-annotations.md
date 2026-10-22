# Annotations

Annotations in Spring are special instructions that we give to Spring using @ symbol.

| Annotation | Use |
|---|---|
| `@Configuration` | Marks a class that declares Spring configuration |

### Dependency Injection Annotations

| Annotation | Use |
|---|---|
| `@Autowired` | Tells Spring to inject a required dependency |
| `@Qualifier` | Specifies which matching bean to inject |
| `@Primary` | Selects a preferred bean when multiple candidates match |
| `@Value` | Injects externalized values from property files (eg: application.properties) into bean fields |
| `@Lazy` | Delays bean initialization until it is first requested, rather than creating it at startup. |
| `@Scope` | Specifies Bean's scope such as singleton or prototype |
| `@PostConstruct` | Runs a initialization method after dependency injection |
| `@PreDestroy` | Run a destruction callback when the bean is being destroyed |

### Spring Web & REST API Annotations

| Annotation | Use |
|---|---|
| `@RestController` | Combines `@Controller` & `@ResponseBody` to mark class as REST API handler where every method returns data (JSON/XML) directly to response body |
| `@RequestMapping` | Maps HTTP request to specific controller paths & methods |
| `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` | Method specific shortcuts for `@RequestMapping` handling various HTTP verbs. |
| `@RequestBody` | Maps an incoming HTTP request body (JSON) directly onto a Java object parameter. |
| `@PathVariable` | Extracts dynamic variables directly out of URI path (eg., `/users/{id}`) |
| `@RequestParam` | Extracts query parameters appended to the URL (eg., `/users?page=2`) |

### Spring Boot Specific Annotation

| Annotation | Use |
|---|---|
| `@SpringBootApplication` | fundamental application anchor. It packages following three annotation |
| `@SpringBootConfiguration` | Tags the class as a specialized configuration source |
| `@EnableAutoConfiguration` | Instructs Spring Boot to automatically assemble beans based on Jar dependencies on classpath. |
| `@ComponentScan` | Dictates the root directory package scanning for stereotype annotations. |

### Data & Framework Management Annotation

| Annotation | Use |
|---|---|
| `@Transactional` | Instructs Spring to execute a group of database operation as one transaction. |
| `@Scheduled` | Schedules a recurring or delayed task (requires `@EnableScheduling` on configuration class) |
