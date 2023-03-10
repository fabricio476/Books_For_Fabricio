


1 - Adicione as dependências do Retrofit no arquivo Gradle do seu projeto:

 ```java
 
  implementation 'com.squareup.retrofit2:retrofit:2.9.0'
  implementation 'com.squareup.retrofit2:converter-gson:2.9.0' // Se você quiser usar Gson como conversor de JSON
  
 ```
 
 * Crie a Interface ApiServise
 
 2 - Crie uma interface para definir a API que você deseja consumir. Essa interface deve conter os métodos que correspondem às chamadas da API, incluindo as anotações de Retrofit.
 
 ```java
 
  public interface MyApi {
    @GET("users/{user}")
    Call<User> getUser(@Path("user") String username);
 }

 ```
 
 * Crie a classe ApiServiceClient 
 
 3 - Crie uma instância do objeto Retrofit usando a classe Retrofit.Builder e defina a URL base da API.
 
 ```java
 
   Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();
 ```
 
 4 - Crie uma instância do serviço de API usando o método create() do Retrofit.
 
 ```java
 
   MyApi myApi = retrofit.create(MyApi.class);

 ```
 
 5 - Faça a chamada da API usando o objeto do serviço de API criado anteriormente e manipule a resposta usando uma instância do objeto Call<T> retornado.
 
 ```java
 
 Call<User> call = myApi.getUser("octocat");
call.enqueue(new Callback<User>() {
    @Override
    public void onResponse(Call<User> call, Response<User> response) {
        if (response.isSuccessful()) {
            User user = response.body();
            // faça algo com os dados do usuário
        } else {
            // tratamento de erros
        }
    }

    @Override
    public void onFailure(Call<User> call, Throwable t) {
        // tratamento de falhas
    }
});

 ```
 
 6 - Certifique-se de adicionar as permissões de Internet no manifesto do seu aplicativo:
 
 ```java
 
 <uses-permission android:name="android.permission.INTERNET" />

 ```
 
 7 - Execute o aplicativo e veja os dados da API sendo exibidos na sua tela.
 
 
 -----------------------------------------------------
 
 * se tiver problema em receber os dados da API , utilize esse código abaixo na classe ApiServiceClient
 
 ```java
 
 
 // PARA desativar temporariamente a verificação SSL ------------------------------------------------

        OkHttpClient.Builder clientBuilder = new OkHttpClient.Builder();

        try {
            // Create a trust manager that does not validate certificate chains
            final TrustManager[] trustAllCerts = new TrustManager[] {
                    new X509TrustManager() {
                        @Override
                        public void checkClientTrusted(java.security.cert.X509Certificate[] chain, String authType) throws CertificateException {
                        }

                        @Override
                        public void checkServerTrusted(java.security.cert.X509Certificate[] chain, String authType) throws CertificateException {
                        }

                        @Override
                        public java.security.cert.X509Certificate[] getAcceptedIssuers() {
                            return new java.security.cert.X509Certificate[]{};
                        }
                    }
            };

            // Install the all-trusting trust manager
            final SSLContext sslContext = SSLContext.getInstance("SSL");
            sslContext.init(null, trustAllCerts, new java.security.SecureRandom());

            // Create an ssl socket factory with our all-trusting manager
            final SSLSocketFactory sslSocketFactory = sslContext.getSocketFactory();

            clientBuilder.sslSocketFactory(sslSocketFactory, (X509TrustManager)trustAllCerts[0]);
            clientBuilder.hostnameVerifier((hostname, session) -> true);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }

        OkHttpClient client = clientBuilder.build();

        // FINAL // PARA desativar temporariamente a verificação SSL ---------------------------------------
 
 
 
 ```
 
 
 
 
 
 
 
 
 
 
 
 
 
