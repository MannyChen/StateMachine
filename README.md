# StateMachine

```cpp
class State{
public:
  state(id = -1) : _state_id(id){}
  virtual ~state() = default;
  
  virtual void init(){};
  virtual void run(){};
  virtual void run(){};

private:
  int32_t _state_id;
}

class StateSche{
public:
  StateSche() = default;
  virtual ~StateSche() = default;
  
  int32_t transitionTo(int32_t id){
    if (id == -1){
      return -1001;
    }
    
    if (id == _cur_state){
      return -1002;
    }
    
    ///< check _cur_state id
    _state_list[_cur_state]->exit();
    _state_list[id]->init();
    
    _cur_state = id;
  }
  
  int32_t tick(){
    ///< check _cur_state
    _state_list[_cur_state]->run();
  }
private:
  int32_t _cur_state = -1;
  std::map<int32_t, State*> _state_list;
}


class 
```

e.g.
```cpp

enum{
  STATE_NONE = -1;
  STATE_LIFT = 1;
  STATE_CATCH = 2;
}

class LiftState final : public State{
public:
  LiftState(id = STATE_LIFT) : State(id){}
  ~LiftState() = default();
  
  void init() override{
    std::out<<"LiftState init"<<std::endl;
  }
  
  void run() override{
    std::out<<"LiftState run"<<std::endl;
  }
  
  void exit() override{
    std::out<<"LiftState exit"<<std::endl;
  }
}

class CatchState final : public State{
public:
  CatchState(id = STATE_CATCH) : State(id){}
  ~CatchState() = default();
  
  void init() override{
    std::out<<"CatchState init"<<std::endl;
  }
  
  void run() override{
    std::out<<"CatchState run"<<std::endl;
  }
  
  void exit() override{
    std::out<<"LiftState exit"<<std::endl;
  }
}

Class  Engineer final : public StateSche{
public: 
  Engineer(){
    StateSche::_state_list.clear();
    StateSche::_state_list.insert({STATE_LIFT, new LiftState()});
    StateSche::_state_list.insert({STATE_CATCH, new CatchState()});
    
    StateSche::transitionTo(STATE_LIFT);
  }
  
  ~Engineer(){
    for(auto& iter : StateSche::_state_list){
      if (nullptr != iter->second){
        delete iter->second;
      }
    }
  }
  
  void EngineerMain({
    StateSche::tick();
  }
}

Enigineer EnigineerImpl;
int main(){
  while(1){
    EnigineerImpl.EngineerMain(); 
  }
}

```
