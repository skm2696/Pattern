// Logger.h - Абстракция
#include <string>
  
// Опережающее объявление
class LoggerImpl;
  
class Logger
{
  public:    
    Logger( LoggerImpl* p );
    virtual ~Logger( );
    virtual void log( string & str ) = 0;
  protected:
    LoggerImpl * pimpl;
};
  
class ConsoleLogger : public Logger
{
  public:    
    ConsoleLogger();
    void log( string & str );
};
  
class FileLogger : public Logger
{
  public:    
    FileLogger( string & file_name );
    void log( string & str );
  private:
    string file;
};
  
class SocketLogger : public Logger
{
  public:    
    SocketLogger( string & remote_host, int remote_port );
    void log( string & str );
  private:
    string host;
    int    port;
};
  
  
// Logger.cpp - Абстракция
#include "Logger.h"
#include "LoggerImpl.h"
  
Logger::Logger( LoggerImpl* p ) : pimpl(p) 
{ }
  
Logger::~Logger( ) 
{ 
    delete pimpl; 
}
  
ConsoleLogger::ConsoleLogger() : Logger( 
      #ifdef MT
        new MT_LoggerImpl()
      #else
        new ST_LoggerImpl()
      #endif
    ) 
{ }
  
void ConsoleLogger::log( string & str ) 
{
    pimpl->console_log( str);
}
  
FileLogger::FileLogger( string & file_name ) : Logger(
      #ifdef MT
        new MT_LoggerImpl()
      #else
        new ST_LoggerImpl()
      #endif
    ), file(file_name) 
{ }
       
void FileLogger::log( string & str ) 
{
    pimpl->file_log( file, str);
}
  
SocketLogger::SocketLogger( string & remote_host, 
                            int remote_port ) : Logger( 
      #ifdef MT
        new MT_LoggerImpl()
      #else
        new ST_LoggerImpl()
      #endif
    ), host(remote_host), port(remote_port) 
{ }
  
void SocketLogger::log( string & str ) 
{
    pimpl->socket_log( host, port, str);
}
  
  
// LoggerImpl.h - Реализация
#include <string>
  
class LoggerImpl
{
  public:    
    virtual ~LoggerImpl( ) {}
    virtual void console_log( string & str ) = 0;
    virtual void file_log( 
                    string & file, string & str ) = 0;
    virtual void socket_log( 
           tring & host, int port, string & str ) = 0;
};
  
class ST_LoggerImpl : public LoggerImpl
{
  public:
    void console_log( string & str );
    void file_log   ( string & file, string & str );
    void socket_log ( 
            string & host, int port, string & str );
};
  
class MT_LoggerImpl : public LoggerImpl
{
  public:
    void console_log( string & str );
    void file_log   ( string & file, string & str );
    void socket_log ( 
            string & host, int port, string & str );
};
  
  
// LoggerImpl.cpp - Реализация
#include <iostream>
#include "LoggerImpl.h"
  
  
void ST_LoggerImpl::console_log( string & str ) 
{
  cout << "Single-threaded console logger" << endl;
}
     
void ST_LoggerImpl::file_log( string & file, string & str ) 
{
  cout << "Single-threaded file logger" << endl;
}
  
void ST_LoggerImpl::socket_log( 
                     string & host, int port, string & str ) 
{
  cout << "Single-threaded socket logger" << endl;
};
  
void MT_LoggerImpl::console_log( string & str ) 
{
  cout << "Multithreaded console logger" << endl;
}
     
void MT_LoggerImpl::file_log( string & file, string & str ) 
{
  cout << "Multithreaded file logger" << endl;
}
     
void MT_LoggerImpl::socket_log( 
                    string & host, int port, string & str ) 
{
  cout << "Multithreaded socket logger" << endl;
}
  
  
// Main.cpp
#include <string>
#include "Logger.h"
  
int main()
{
  Logger * p = new FileLogger( string("log.txt"));
  p->log( string("message"));
  delete p;        
  return 0;
}
