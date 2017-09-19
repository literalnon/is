#include <iostream>
#include <math.h>

int getOp(int s, int e){
    int i = 0;

    if(e <= s){
        return i;
    } else if(e % 2 == 0){
        i = getOp(s, e / 2) + 1;
        std::cout << i << ": " << e/2 << " * 2 = " << e << std::endl;
    }else if(e > s){
        i = getOp(s, e - 3) + 1;
        std::cout << i << ": " << e - 3 << " + 3 = " << e <<std::endl;
    }

    return i;
}

int main(){
    int s = 2, e = 5000;
\
    std::cout << getOp(s, e);

    return 0;
}
