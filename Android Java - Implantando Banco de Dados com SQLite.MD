

1 -  Crie uma classe DBHelper ( DAO ) que estenda a classe SQLiteOpenHelper e implemente os métodos onCreate() e onUpgrade(). Na implementação do onCreate(), você criará suas tabelas e, na implementação do onUpgrade(), você realizará as atualizações necessárias no esquema do banco de dados.


```java

public class DBHelper extends SQLiteOpenHelper {
    private static final int DATABASE_VERSION = 1;
    private static final String DATABASE_NAME = "my_db";

    public DBHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String createTableQuery = "CREATE TABLE my_table (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, age INTEGER)";
        db.execSQL(createTableQuery);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        String dropTableQuery = "DROP TABLE IF EXISTS my_table";
        db.execSQL(dropTableQuery);
        onCreate(db);
    }
}



```


2 - Crie uma classe para lidar com as operações CRUD (Create, Read, Update e Delete) no banco de dados. Nessa classe, você usará métodos da classe SQLiteDatabase para executar as operações.

```java

public class DBHandler {
    private DBHelper dbHelper;

    public DBHandler(Context context) {
        dbHelper = new DBHelper(context);
    }

    public void addData(String name, int age) {
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put("name", name);
        values.put("age", age);
        db.insert("my_table", null, values);
        db.close();
    }

    public List<Data> getAllData() {
        List<Data> dataList = new ArrayList<>();
        String selectQuery = "SELECT * FROM my_table";
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        Cursor cursor = db.rawQuery(selectQuery, null);
        if (cursor.moveToFirst()) {
            do {
                int id = cursor.getInt(cursor.getColumnIndex("id"));
                String name = cursor.getString(cursor.getColumnIndex("name"));
                int age = cursor.getInt(cursor.getColumnIndex("age"));
                Data data = new Data(id, name, age);
                dataList.add(data);
            } while (cursor.moveToNext());
        }
        cursor.close();
        db.close();
        return dataList;
    }

    public void updateData(int id, String name, int age) {
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put("name", name);
        values.put("age", age);
        db.update("my_table", values, "id = ?", new String[] {String.valueOf(id)});
        db.close();
    }

    public void deleteData(int id) {
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        db.delete("my_table", "id = ?", new String[] {String.valueOf(id)});
        db.close();
    }
}



```
 
3 - Use a classe DBHandler em sua atividade para executar as operações CRUD.

```java

public class MainActivity extends AppCompatActivity {
    private DBHandler dbHandler;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        dbHandler = new DBHandler(this);

        // Adicionando dados
        dbHandler.addData("John Doe", 25);
        dbHandler.addData("Jane Doe", 30);

        // Obtendo todos os dados
        List<Data> dataList = dbHandler.getAllData();
        
        for (Data data : dataList) {
            Log.d("MainActivity", "ID: " + data.getId() + ", Name: " + data.getName() + ", Age: " + data.getAge());
        }

        // Atualizando dados
        dbHandler.updateData(1, "John Smith", 26);

        // Deletando dados
        dbHandler.deleteData(2);
    }
}

```




