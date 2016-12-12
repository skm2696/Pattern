// Интерфайс абстракции
class AlarmClock {
private:
  virtual void toWake() = 0;
protected:
  /**
    It`s our bridge to implementation
  */
  AlarmClockImpl *bridge;
public:

  virtual void start() = 0;
  virtual void stop() = 0;
  
};

// Интерфейс реализации
class AlarmClockImpl {
public:
  virtual void ring() = 0;
  virtual void notify() = 0;
};

// Уточнение абстракции будильника в качестве зависающего
class LockupAlarmClock : public AlarmClock {
private:
  WORD hourAlarm;    // час расплаты
  WORD minutesAlarm;  // минута расплаты
  bool waitForWake;   // флаг признака ожидания

  virtual void toWake();
protected:
public:
  LockupAlarmClock(AlarmClockImpl& bridgeImpl, WORD hour, WORD minutes);

  virtual void start();
  virtual void stop();
};

LockupAlarmClock::LockupAlarmClock(AlarmClockImpl& bridgeImpl, WORD hour, WORD minutes) {
  this->bridge = &bridgeImpl;
  this->waitForWake = false;
  
  this->hourAlarm = hour;
  this->minutesAlarm = minutes;
}

// Вставай!!! Вставай!!
void LockupAlarmClock::toWake() {
  this->bridge->notify();
  this->bridge->ring();
}

// Запускаем процесс "зависания" будильника
void LockupAlarmClock::start() {
  // start lockup process
  SYSTEMTIME time;
  waitForWake = true;

  while (waitForWake) {
      
    GetLocalTime(&time);

    if (time.wHour == this->hourAlarm && time.wMinute == this->minutesAlarm) {
      waitForWake = false;
    }

    Sleep(100);
  }  

  toWake();
}

void LockupAlarmClock::stop() {
  // stop lockup process
  waitForWake = false;
}

// Уточнение интерфейса реализации в качестве будильника, играющего MP3 через внешнюю команду
class ShellMP3AlarmClock : public AlarmClockImpl {
private:
  string cmdplay; // сама команда
protected:
public:
  ShellMP3AlarmClock(const string& cmd);
  ~ShellMP3AlarmClock();

  virtual void ring();
  virtual void notify();
  
};

ShellMP3AlarmClock::ShellMP3AlarmClock(const string& cmd) {
  this->cmdplay = cmd;
}

void ShellMP3AlarmClock::ring() {
  // run command
  system(cmdplay.c_str());
}

void ShellMP3AlarmClock::notify() {
  cout << "ALARMING!" << endl;
}

