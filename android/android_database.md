Database


##ContentProvider
[ContentProvider](http://blog.csdn.net/worker90/article/details/7016430)

##ContentObserver
[Android中内容观察者的使用---- ContentObserver类详解 （转）
](http://www.cnblogs.com/slider/archive/2012/02/14/2351702.html)


##SQLite

[Android学习笔记之SQLite
](http://www.blogjava.net/mixer-a/archive/2012/01/30/375046.html)

1：定义表的结构和名字，我使用以下方法：

public interface Constatnts extends BaseColumns {
   public static final String TABLE_NAME = "test";
   
   public static final String TIME = "time";
   public static final String TITLE = "title";
}

在这里，我继承BaseColumns的目的，是直接定义"_ID"字段

 

2：定义SQLiteOpenHelper 的子类，来对应一个数据库，重载必须的onCreate 和 onUpgrade。里面通过execSQL调用适当的SQL 语句。

 

 

@Override

// 这个函数会在第一次执行数据库操作的时候被调用到。


 public void onCreate(SQLiteDatabase db) {
  // TODO Auto-generated method stub 
  db.execSQL("CREATE TABLE " + TABLE_NAME + " (" + _ID
             + " INTEGER PRIMARY KEY AUTOINCREMENT, " + TIME
             + " INTEGER," + TITLE + " TEXT NOT NULL);");  //注意，最好声明字段类型，以免不必要的麻烦。
 } 

 @Override
 public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
  // TODO Auto-generated method stub
        db.execSQL("DROP TABLE IF EXITS "+ TABLE_NAME);
        onCreate(db);
 }

 

3：使用该数据库。

   mydatabase = new Databasehelp(this);

插入数据：

SQLiteDatabase db = mydatabase.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put(TIME, System.currentTimeMillis());
        values.put(TITLE, string);
        db.insertOrThrow(TABLE_NAME, null, values);

 

 

查询数据：

SQLiteDatabase db = mydatabase.getReadableDatabase();
        Cursor cursor = db.query(TABLE_NAME, { _ID, TIME, TITLE, }, null, null, null,
              null, null);
        startManagingCursor(cursor); // 让activity来管理cursor的生命周期

while (cursor.moveToNext()) { 
           // Could use getColumnIndexOrThrow() to get indexes
           long id = cursor.getLong(0); 
           long time = cursor.getLong(1);
           String title = cursor.getString(2);

          ...........................................

}

 

4: 直接管理数据库

通过adb shell进入：sqlite3 test.db，后面按照标准的sqlites 办法做。

 

引用一个好总结：

使用数据库:sqlite3 db_name
创建表： create table table_name(filed1Name filed1Property,filed2Name filed2Property);
显示数据库中的表：.table
显示表结构：.schema table_name
删除表: .drop table table_name  // 我没有找到这个命令，呵呵
退出：.exit
导入数据：将txt文件中的数据到导入到表中, data.txt中每一列用”|”分隔, |两边不要空格,文件末尾不要有空行，例如：1|rainkey|tencent
注意：在Android 上面执行的时候将text_name.txt存放到databases所在目录
.import text_name.txt table_name
插入记录：insert into table_name values(‘key’,’value’);
查找记录：select * from table_name;
从第三行开始查询10条记录：select * from table_name limit 3,10;
删除记录：delete from table_name where field1=value;
更新操作：update table_name set name=’rainkey’ where id=1;
另外，如果要重新刷新这个数据的库的话，我现在的方法是通过adb uninstall整个程序。