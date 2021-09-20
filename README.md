# todo-springboot


## todo-testing
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
