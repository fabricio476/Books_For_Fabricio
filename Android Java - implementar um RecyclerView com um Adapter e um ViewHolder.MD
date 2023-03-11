
Segue abaixo um exemplo de como implementar um RecyclerView com um Adapter e um ViewHolder em um projeto Android com Java:


1 - Adicione a dependência do RecyclerView no arquivo build.gradle do seu projeto:

```Java

implementation 'androidx.recyclerview:recyclerview:1.2.1'


```

2 - Crie um layout XML para o item da lista que será exibido no RecyclerView. Por exemplo, crie um arquivo chamado list_item.xml na pasta res/layout:

```Java

<TextView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/item_text_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp"
    android:textSize="24sp" />


```

3 - Crie um layout XML para o RecyclerView. Por exemplo, crie um arquivo chamado activity_main.xml na pasta res/layout:


```Java

<?xml version="1.0" encoding="utf-8"?>
<androidx.recyclerview.widget.RecyclerView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/recycler_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />

```

4 - Crie uma classe para representar os dados que serão exibidos no RecyclerView. Por exemplo, crie uma classe chamada ListItem:

```Java

public class ListItem {
    private String text;

    public ListItem(String text) {
        this.text = text;
    }

    public String getText() {
        return text;
    }
}


```

5 - Crie uma classe que estenda a classe RecyclerView.Adapter e implemente o método onCreateViewHolder, que cria um novo ViewHolder para exibir os itens da lista, e o método onBindViewHolder, que atualiza o ViewHolder com os dados do item correspondente. Por exemplo, crie uma classe chamada ListAdapter:

```Java
public class ListAdapter extends RecyclerView.Adapter<ListAdapter.ViewHolder> {

    private List<ListItem> items;

    public ListAdapter(List<ListItem> items) {
        this.items = items;
    }

    public class ViewHolder extends RecyclerView.ViewHolder {
        private TextView textView;

        public ViewHolder(View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.item_text_view);
        }

        public void bind(ListItem item) {
            textView.setText(item.getText());
        }
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.list_item, parent, false);
        return new ViewHolder(itemView);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        ListItem item = items.get(position);
        holder.bind(item);
    }

    @Override
    public int getItemCount() {
        return items.size();
    }
}


```

6 - Na classe da Activity ou Fragment que irá exibir o RecyclerView, defina um LayoutManager, crie uma lista de itens e crie um Adapter com a lista de itens. Por exemplo, na classe MainActivity:

 ```Java
 
 public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        recyclerView.setLayoutManager(new LinearLayoutManager(this));

        List<ListItem> items = new ArrayList<>();
        items.add(new ListItem("Item 1"));
        
        
        }
       } 
        
 ```

7 - Crie uma instancia do ListAdapter e atribua-a ao RecyclerView:

```Java
  ListAdapter adapter = new ListAdapter(items);
        recyclerView.setAdapter(adapter);

```

8 - Execute o aplicativo e verifique se o RecyclerView está exibindo corretamente os itens da lista.
     O código completo para a classe ListAdapter e a classe MainActivity ficaria assim:
     
   ```Java  
     
     public class ListAdapter extends RecyclerView.Adapter<ListAdapter.ViewHolder> {

    private List<ListItem> items;

    public ListAdapter(List<ListItem> items) {
        this.items = items;
    }

    public class ViewHolder extends RecyclerView.ViewHolder {
        private TextView textView;

        public ViewHolder(View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.item_text_view);
        }

        public void bind(ListItem item) {
            textView.setText(item.getText());
        }
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View itemView = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.list_item, parent, false);
        return new ViewHolder(itemView);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        ListItem item = items.get(position);
        holder.bind(item);
    }

    @Override
    public int getItemCount() {
        return items.size();
    }
}

     
   ```  
   
   
   ```Java
   
   public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        recyclerView.setLayoutManager(new LinearLayoutManager(this));

        List<ListItem> items = new ArrayList<>();
        items.add(new ListItem("Item 1"));
        items.add(new ListItem("Item 2"));
        items.add(new ListItem("Item 3"));

        ListAdapter adapter = new ListAdapter(items);
        recyclerView.setAdapter(adapter);
    }
}

   ```
   
   Lembre-se de adicionar as permissões no manifest.xml para acessar a internet, caso a lista seja carregada de uma fonte remota.
     
     
     
     
     


































