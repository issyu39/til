## Retrofit
A type-safe HTTP client for Android and Java

### Document
https://square.github.io/retrofit/

### 導入
#### Serviceの定義
```Java
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user, @Query("sort") String sort);
}
```
- HTTPのメソッドに対応するアノテーションは一通り揃っている(`@GET`, `@POST`, `@PUT`, `@DELETE`, `@HEAD`, `@PATCH`)

#### Serviceのインスタンスを生成
```Java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```
### RequestBodyでの指定
```Java
@POST("users/new")
Call<User> createUser(@Body User user);
```

### `application/x-www-form-urlencoded`形式でのPOST
```Java
@FormUrlEncoded
@POST("user/edit")
Call<User> updateUser(@Field("first_name") String first, @Field("last_name") String last);
```

### Headerの挿入
```Java
@Headers({
    "Accept: application/vnd.github.v3.full+json",
    "User-Agent: Retrofit-Sample-App"
})
@GET("users/{username}")
Call<User> getUser(@Path("username") String username);
```
