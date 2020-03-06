# Catalog
* [Add dll](#add-dll)
* [QTextBrowser](#QTextBrowser)
* [QTimer::singleShot](#QTimersingleShot)
* [Qt::BlockingQueuedConnection](#QtBlockingQueuedConnection)
* [QSqlDatabase - MySql](#QSqlDatabase---MySql)


## add dll   
```c++
LIBS += $$PWD/xxx.dlll
```  
[Top](#Catalog) 
***
## QTextBrowser
```c++
void write_msg(QTextBrowser *text, QString msg, const QColor c)
{
    text->moveCursor(QTextCursor::End);
    text->setTextColor(c);
    text->insertPlainText("[" + QTime::currentTime().toString(Qt::ISODate) + "] " + msg);
    text->setTextColor(Qt::black);
    text->insertPlainText("\n");
    text->moveCursor(QTextCursor::End);
}
```
[Top](#Catalog)   
***
## QTimer::singleShot
```c++
[static] void QTimer::singleShot(int msec, const QObject *receiver, const char *member)
```
This static function calls a slot after a given time interval.  
Example:
```c++
QTimer::singleShot(1000, this, SLOT(xxx_func()));  
```
[Top](#Catalog)   
***
## Qt::BlockingQueuedConnection
Same as Qt::QueuedConnection, except that the signalling thread blocks until the slot returns. This connection must not be used if the receiver lives in the signalling thread, or else the application will deadlock.  
[reference] https://ramihaha.tw/program-qt-connection-multithread/  
[Top](#Catalog) 
***
## QSqlDatabase - MySql
```c++
    QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
    db.setHostName("xxx"); /*IP*/
    db.setDatabaseName("xxx");
    db.setUserName("xxx");
    db.setPassword("xxx");
    if(!db.open())
    {   
        /*open fail...*/
        return ;
    }
    QSqlQuery *query = new QSqlQuery(db);
    query->exec("xxx"); /*sql command*/
    
    db.close();
```
[Top](#Catalog)  
***
## QFileDialog
- Select Files
```cpp
QString qsFile = QFileDialog::getOpenFileName(this, "Select Update File", QDesktopServices::storageLocation(QDesktopServices::DesktopLocation), "*.zip");
QStringList qslFile = QFileDialog::getOpenFileNames...
```
- Save File
```cpp
QString qsFileName = QFileDialog::getSaveFileName(this, "Save Test Scrip", QApplication::applicationDirPath(), "*.clscrip", 0);

```
- Select Folder
```cpp
QString qsDir = QFileDialog::getExistingDirectory...

```
[Top](#Catalog)  
***

