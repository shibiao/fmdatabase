# fmdatabase
简单的创建database和对database进行增删改查的操作
<p>
项目操作前,先导入fmdb第三方库
</p>
1.首先创建数据库
<pre><code>
{
    //1.创建数据库
    //ios数据库一般会保存在沙盒文件的Documents中
    NSArray *documents=NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
    //    NSLog(@"%@",NSHomeDirectory());
    NSString *documentsPath=[documents firstObject];
    NSString *dbPath=[documentsPath stringByAppendingPathComponent:@"test.db"];
    NSLog(@"%@",dbPath);
    _dataBase=[FMDatabase databaseWithPath:dbPath];
    if ([_dataBase open]) {
        NSLog(@"打开成功");
    }else{
        NSLog(@"打开失败");
    }
    
    //2.创建表
    NSString *createTableSql=@"create table if not exists Person(id integer primary key autoincrement,name text,age integer)";
    //fmdb对数据库操作分为
    BOOL success=[_dataBase executeUpdate:createTableSql];
    if (success) {
        NSLog(@"创建表成功");
    }else{
        NSLog(@"创建表失败");
    }
    //3.增删改查
    [self insert];
    [self update];
    [self delete];
    //关闭数据库
    [_dataBase close];
    }
    </code></pre>
    
    2.增删改查
 <pre><code>
    {-(void)insert//插入数据
{
    NSString *insertSQL=@"insert into Person (name,age) values (?,?)";
    BOOL success=[_dataBase executeUpdate:insertSQL,@"王五",@19];
    if (success) {
        NSLog(@"插入成功");
    }else{
        NSLog(@"插入失败");
    }
}
-(void)update//改数据
{
    NSString *updateSQL=@"update Person set name=?,age=? where id=?";
    BOOL success=[_dataBase executeUpdate:updateSQL,@"王五",@20,@1];
    if (success) {
        NSLog(@"更新成功");
    }else{
        NSLog(@"更新失败");
    }
}
-(void)delete//删除数据
{
    NSString *deleteSQL=@"delete from Person where age=? ";
    BOOL success=[_dataBase executeUpdate:deleteSQL,@20];
    if (success) {
        NSLog(@"删除成功");
    }else{
        NSLog(@"删除失败");
    }
}
-(void)select//选择数据
{
    NSString *selectSQL=@"select name,age from Person";
    BOOL success=[_dataBase executeUpdate:selectSQL];
    if (success) {
        NSLog(@"选择成功");
    }else{
        NSLog(@"选择失败");
    }
}} 
</code></pre>
