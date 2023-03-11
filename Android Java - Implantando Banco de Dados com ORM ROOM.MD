
# Minha Lista de Tarefas

Este é um aplicativo de lista de tarefas




1 - Adicione as dependências do Room em seu arquivo build.gradle (Módulo app):

```Java

dependencies {
    def room_version = "2.4.0"
    implementation "androidx.room:room-runtime:$room_version"
    annotationProcessor "androidx.room:room-compiler:$room_version"
    // (Opcional) Biblioteca de suporte para RxJava
    implementation "androidx.room:room-rxjava2:$room_version"
    // (Opcional) Biblioteca de suporte para Kotlin
    implementation "androidx.room:room-ktx:$room_version"
}



```

2 - Crie uma entidade para representar uma tarefa. Para isso, crie uma classe com o nome de Task e adicione as seguintes anotações:

```Java

@Entity(tableName = "tasks")
public class Task {
    @PrimaryKey(autoGenerate = true)
    private int id;
    private String title;
    private String description;
    private boolean isCompleted;

    // Construtor
    public Task(String title, String description, boolean isCompleted) {
        this.title = title;
        this.description = description;
        this.isCompleted = isCompleted;
    }

    // Getters e Setters
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public boolean isCompleted() {
        return isCompleted;
    }

    public void setCompleted(boolean completed) {
        isCompleted = completed;
    }
}



```

3 - Crie uma interface para acessar o banco de dados. Para isso, crie uma interface com o nome de TaskDao e adicione os métodos CRUD:

```Java

@Dao
public interface TaskDao {
    @Insert
    Completable insert(Task task);

    @Update
    Completable update(Task task);

    @Delete
    Completable delete(Task task);

    @Query("SELECT * FROM tasks")
    Single<List<Task>> getAllTasks();
}


```

4 - Crie uma classe abstrata para criar o banco de dados. Para isso, crie uma classe com o nome de AppDatabase e adicione as seguintes anotações:


```Java 

@Database(entities = {Task.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract TaskDao taskDao();
}


```


5 - Instancie o banco de dados em sua activity ou fragment. Para isso, crie uma variável global:

```Java

private AppDatabase db;


```

6 - Inicialize o banco de dados em onCreate da activity ou onCreateView do fragment:

```Java

db = Room.databaseBuilder(getApplicationContext(), AppDatabase.class, "task-db").build();


```

7 - Adicione uma nova tarefa:

```Java

Task task = new Task("Minha tarefa", "Descrição da minha tarefa", false);
db.taskDao().insert(task)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(() -> {
        // Tarefa adicionada com sucesso
    }, throwable -> {
        // Erro ao adicionar tarefa
    });


```

8 - Obtenha todas as tarefas:

```Java

db.taskDao().getAllTasks()
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(tasks -> {
        // Lista de tarefas obtida com sucesso
    }, throwable -> {
        // Erro ao obter lista de tarefas
    });


```

9 - Atualize uma tarefa:

```Java
Task task = new Task("Minha tarefa atualizada", "Descrição da minha tarefa atualizada", true);
task.setId(1); // Defina o ID da tarefa que deseja atualizar
db.taskDao().update(task)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(() -> {
        // Tarefa atualizada com sucesso
    }, throwable -> {
        // Erro ao atualizar tarefa
    });


```

10 - Exclua uma tarefa:

```Java

Task task = new Task("", "", false);
task.setId(1); // Defina o ID da tarefa que deseja excluir
db.taskDao().delete(task)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(() -> {
        // Tarefa excluída com sucesso
    }, throwable -> {
        // Erro ao excluir tarefa
    });



```
11 - Adicione comentários ao código para facilitar a compreensão e manutenção futura:

```Java

// Criação de objeto de banco de dados
db = Room.databaseBuilder(getApplicationContext(), AppDatabase.class, "task-db").build();

// Adição de uma nova tarefa
Task task = new Task("Minha tarefa", "Descrição da minha tarefa", false);
db.taskDao().insert(task)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(() -> {
        // Tarefa adicionada com sucesso
    }, throwable -> {
        // Erro ao adicionar tarefa
    });

// Obtenção de todas as tarefas
db.taskDao().getAllTasks()
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(tasks -> {
        // Lista de tarefas obtida com sucesso
    }, throwable -> {
        // Erro ao obter lista de tarefas
    });

// Atualização de uma tarefa
Task task = new Task("Minha tarefa atualizada", "Descrição da minha tarefa atualizada", true);
task.setId(1); // Defina o ID da tarefa que deseja atualizar
db.taskDao().update(task)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(() -> {
        // Tarefa atualizada com sucesso
    }, throwable -> {
        // Erro ao atualizar tarefa
    });

// Exclusão de uma tarefa
Task task = new Task("", "", false);
task.setId(1); // Defina o ID da tarefa que deseja excluir
db.taskDao().delete(task)
    .subscribeOn(Schedulers.io())
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(() -> {
        // Tarefa excluída com sucesso
    }, throwable -> {
        // Erro ao excluir tarefa
    });



```





































