#include <iostream>

using namespace std;

class op{
public:
    static const int MAX_PRIORITY = 2;
    static const int MIN_PRIORITY = 1;

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

class node{
public:
    bool first_op;
    bool second_op;

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

    bool operation(){
        if(first_op == false){
            value = mult.operation(value);
            first_op = true;
            return true;
        } else if(second_op == false){
            value = sum.operation(value);
            second_op = true;
            return true;
        } else {
            return false;
        }
    }

    string string_op(){
        if(!second_op){
            return "+";
        } else {
            return "*";
        }
    }

};

int solution(int start_value, int finish){

    node * cur_node = new node(NULL, start_value);

    while(cur_node->value != finish){

        if(cur_node->value < finish){

            if(cur_node->operation()){
                cur_node = new node(cur_node, cur_node->value);
            } else {
                cur_node = cur_node->parent;
            }
        } else if(cur_node->value > finish){
            cur_node = cur_node->parent;
        }

    }

    while(cur_node->parent != NULL){
        std::cout << cur_node->string_op() << std::endl;
        cur_node = cur_node->parent;
    }

    return cur_node->value;
}


int main(int argc, char *argv[])
{
    std::cout << solution(2, 8) << std::endl;

    return 0;
}
