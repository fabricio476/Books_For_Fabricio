


usar o padrão de projeto Singleton para gerenciar o acesso ao SharedPreferences em todo o seu aplicativo Android, usando o Gson para serializar e desserializar objetos. Além disso, refatoramos o método saveObject para usar o método apply() em vez de commit(), que é uma prática recomendada para melhorar o desempenho do aplicativo. Também adicionamos o modificador synchronized ao método getInstance para garantir que ele seja thread-safe.


```Java

// implementar na classe do objeto que vai ser convertida usando a GSON
implements Serializable

```



1 - Adicione a dependência do Gson ao arquivo build.gradle do seu projeto:

```Java

implementation 'com.google.code.gson:gson:2.8.7'


```

2 - Crie uma nova classe chamada SharedPreferencesManager e torne-a um Singleton:

```Java

public class SharedPreferencesManager {

    private static SharedPreferencesManager instance;
    private SharedPreferences sharedPreferences;
    private Gson gson;

    private SharedPreferencesManager(Context context) {
        sharedPreferences = context.getSharedPreferences("MySharedPreferences", Context.MODE_PRIVATE);
        gson = new Gson();
    }

    public static synchronized SharedPreferencesManager getInstance(Context context) {
        if (instance == null) {
            instance = new SharedPreferencesManager(context);
        }
        return instance;
    }

    // Métodos para salvar e recuperar valores do SharedPreferences usando Gson

}


```




3 - Adicione os métodos necessários para salvar e recuperar valores do SharedPreferences usando o Gson. Por exemplo, você pode adicionar um método para salvar um objeto:


```Java

public <T> void saveObject(String key, T object) {
    String json = gson.toJson(object);
    sharedPreferences.edit().putString(key, json).apply();
}

```

4 - Adicione métodos para recuperar valores do SharedPreferences usando o Gson. Por exemplo, você pode adicionar um método para recuperar um objeto:

```Java

public <T> T getObject(String key, Class<T> type) {
    String json = sharedPreferences.getString(key, "");
    return gson.fromJson(json, type);
}


```

5 - Para usar o SharedPreferencesManager com o Gson, obtenha uma instância usando o método getInstance() e chame os métodos necessários:

```Java

SharedPreferencesManager sharedPreferencesManager = SharedPreferencesManager.getInstance(context);
User user = new User("JohnDoe", 30);
sharedPreferencesManager.saveObject("user", user);
User savedUser = sharedPreferencesManager.getObject("user", User.class);


```











