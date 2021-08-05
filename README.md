# StateMachine

1. 伪代码，使用自行修编译错误
2. 该检查的指针自行增加检查

```cpp
class State{
public:
  State(id = -1) : _state_id(id){}
  virtual ~State() = default;
  
  virtual void init(){};
  virtual int32_t run(){};
  virtual void exit(){};

private:
  int32_t _state_id = -1;
};

class StateSche{
public:
  StateSche() = default;
  virtual ~StateSche() = default;
  
  int32_t transitionTo(int32_t id){
    if (id == -1){
      return -1001; ///< id not valid
    }
    
    if (id == _cur_state){
      return -1002; ///< _cur_state is running
    }
    
    auto iter = _state_list.find(_cur_state);
    if (iter == _state_list.end()){
      return -1001; ///< _cur_state is not valid
    }
    _state_list[_cur_state]->exit();
    _cur_state = -1;
    
    iter = _state_list.find(id);
    if (iter == _state_list.end()){
      return -1001; ///< id is not valid
    }
    _state_list[id]->init();
    
    _cur_state = id;
    
    return 0;
  }
  
  int32_t tick(){
    ///< check _cur_state
    return _state_list[_cur_state]->run();
  }
private:
  int32_t _cur_state = -1;
  std::map<int32_t, State*> _state_list;
};
```

e.g.
```cpp

enum{
  STATE_NONE = -1，
  STATE_LIFT = 1，
  STATE_CATCH = 2，
};

class LiftState final : public State{
public:
  LiftState(id = STATE_LIFT) : State(id){}
  ~LiftState() = default;
  
  void init() override{
    std::out<<"LiftState init"<<std::endl;
  }
  
  int32_t run() override{
    std::out<<"LiftState run"<<std::endl;
  }
  
  void exit() override{
    std::out<<"LiftState exit"<<std::endl;
  }
};

class CatchState final : public State{
public:
  CatchState(id = STATE_CATCH) : State(id){}
  ~CatchState() = default;
  
  void init() override{
    std::out<<"CatchState init"<<std::endl;
  }
  
  int32_t run() override{
    std::out<<"CatchState run"<<std::endl;
  }
  
  void exit() override{
    std::out<<"LiftState exit"<<std::endl;
  }
};

class Engineer final : public StateSche{
public: 
  Engineer(){
    StateSche::_state_list.clear();
    StateSche::_state_list.insert({STATE_LIFT, new LiftState()});
    StateSche::_state_list.insert({STATE_CATCH, new CatchState()});
    
    StateSche::transitionTo(STATE_LIFT);
  }
  
  ~Engineer(){
    for(auto& iter : StateSche::_state_list){
      if (nullptr != iter.second){
        delete iter.second;
      }
    }
  }
  
  void EngineerMain({
    StateSche::tick();
  }
};

Enigineer EnigineerImpl;
int main(){
  while(1){
    EnigineerImpl.EngineerMain(); 
  }
  
  return 0;
}

```
