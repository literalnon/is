#include <iostream>

using namespace std;

class op{ public: static const int MAX_PRIORITY = 2; static const int MIN_PRIORITY = 1;

int priority;
int digit;

op(char op, int digit){
    this->digit = digit;

    switch (op) {
    case '+':
        priority = MIN_PRIORITY;
        break;
    case '*':
        priority = MAX_PRIORITY;
        break;
    }
}

int operation(int s_digit){
    if(priority == MAX_PRIORITY){
        return s_digit * digit;
    } else { //if(priority == MIN_PRIORITY)
        return s_digit + digit;
    }
}

int antiOp(int f_digit){
    if(priority == MAX_PRIORITY){
        return (int)(f_digit / digit);
    }else if(priority == MIN_PRIORITY){
        return (int)(f_digit - digit);
    }
}

};

class node{ public: bool first_op; bool second_op;

int value;
node * parent;

op sum = op('+', 3);
op mult = op('*', 2);

node(node * parent){
    this -> parent = parent;

    first_op = false;
    second_op = false;
}

node(node * parent, int value){
    std::cout << value << std::endl;

    this -> parent = parent;
    this->value = value;

    first_op = false;
    second_op = false;
}


int operation(){
    if(first_op == false){
        int val = mult.operation(value);
        first_op = true;
        return val;
    } else if(second_op == false){
        int val = sum.operation(value);
        second_op = true;
        return val;
    }
}

    bool isValidate(){
        return !(first_op && second_op);
    }

string string_op(){
    if(!second_op){
        return "+" + value;
    } else {
        return "*" + value;
    }
}

};

int solution(int start_value, int finish){

node * cur_node = new node(NULL, start_value);

while(cur_node->value != finish){

    if(cur_node->value < finish){

        if(cur_node->isValidate()){
            cur_node = new node(cur_node, cur_node->operation());
        } else {
            cur_node = cur_node->parent;
        }
    } else if(cur_node->value > finish){
        cur_node = cur_node->parent;
    }else{
        break;
    }

}

int i = 0;
while(cur_node->parent != NULL){
    std::cout << i++ << cur_node->string_op() << std::endl;
    cur_node = cur_node->parent;
}

return cur_node->value;

}

int main(int argc, char *argv[]) {
    std::cout << solution(2, 20) << std::endl;

    return 0;
}
