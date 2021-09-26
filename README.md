# todo-springboot


## todo-testing

### Service Test
```java
public class TaskServiceTest {

    @Mock
    TodoValidator todoValidator;

    @Mock
    TodoRepo todoRepository;

    private TodoService taskService;

    @Captor
    ArgumentCaptor<TodoItem> todoItemArgumentCaptor;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
        taskService = new TodoService(todoValidator, todoRepository);
    }



    @Test
    void itShouldAddNewTodoTask() {
        // given
        String title = "Interview Test";
        TodoItem todoItem = new TodoItem(title, LocalDate.now(), "some desription", Status.PENDING);

        given(todoRepository.findByTitle(title)).willReturn(Optional.empty());
        given(todoValidator.validateTitleLength(title)).willReturn(true);

        // when
        taskService.addTask(todoItem);

        // then
        then(todoRepository).should().save(todoItemArgumentCaptor.capture());
        TodoItem todoItemCaptor = todoItemArgumentCaptor.getValue();

        assertThat(todoItemCaptor).isEqualTo(todoItem);
    }
}
```

### Controller Test

```java

@ExtendWith( SpringExtension.class)
@SpringBootTest
@AutoConfigureMockMvc
public class TestControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void getsAllEntries() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders.get("/test")
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andReturn();
    }

}

```

### Utils class for object to json string converter

```java

public class TestUtils {
    public static String asJsonString(final Object obj) {
        try {
            final ObjectMapper mapper = new ObjectMapper();
            final String jsonContent = mapper.writeValueAsString(obj);
            return jsonContent;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```
