读ViewModule的记录

### 有趣的注释

1. 控制变量的注释

   ```java
   to handle active/inactive reentry, we guard with this boolean
   ```

   注：to handle .......,we guard with this boolean  语式

2. SuppressWarning

   ```java
   @SuppressWarnings("WeakerAccess")
   ```

   注：禁止“Access can be private”的警告

3. ```java
   @MainThread  @Nullable
   ```

   注：写代码时可以加这些注释

4.把iosException封装成RunTimeException

注：IOException是已检查的异常，无法主动抛出。 RuntimeException，可以从任何方法中抛出。

通常在无法从本地恢复检查的异常，可以在RuntimeException中包装检查的异常。

一旦将检查的异常包装在RuntimeException中，RuntimeException便可以一直传播到整个堆栈，不管是否存在异常声明。