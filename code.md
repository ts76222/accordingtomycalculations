#include <iostream>
#include <stack>
#include <string>
#include <cctype>
using namespace std;

class Calculator {
public:
    double calc(string expression) {
        string postfix = infixpostfix(expression);
        return calcpostfix(postfix);
    }

private:
    int precedence(char ab) {
        if (ab == '+' || ab == '-')
        return 1;
        if (ab == '*' || ab == '/')
        return 2;
        
        return 0;
    }
    string infixpostfix(string expression) {
        stack<char> operators;
        string output = "";
        for (int i = 0; i < expression.length(); i++) {
            char x = expression[i];
            if (isdigit(x) || x == '.') {
            while (i < expression.length() &&
            (isdigit(expression[i]) || expression[i] == '.')) {
            output += expression[i];
            i++;
            }
            output += ' ';
            i--;
        }
            
        else if (x == '(') {
            operators.push(x);
        }   
        else if (x == ')') {
            while (!operators.empty() &&
            operators.top() != '(') {
            output += operators.top();
            output += ' ';
            operators.pop();
        }
            operators.pop();
        }   
        else {
        while (!operators.empty() &&
            operators.top() != '(' &&
            precedence(operators.top()) >= precedence(x)) {
            output += operators.top();
            output += ' ';
            operators.pop();
        }
            operators.push(x);
        }
    }
        while (!operators.empty()) {
            output += operators.top();
            output += ' ';
            operators.pop();
        }
        return output;
    }
    double calcpostfix(string postfix) {
        stack<double> numbers;
        string number = "";
        for (int i = 0; i < postfix.length(); i++) {
        char x = postfix[i];
        if (isdigit(x) || x == '.') {
        number += x;
    }
        else if (x == ' ') {
        if (!number.empty()) {
        numbers.push(stod(number));
        number = "";
    }
}
        else {
        double y = numbers.top();
        numbers.pop();
        double z = numbers.top();
        numbers.pop();
        if (x == '+')
        numbers.push(z+y);
        else if (x == '-')
        numbers.push(z-y);
        else if (x == '*')
        numbers.push(z*y);
        else if (x == '/')
        numbers.push(z/y);
    }
}
    return numbers.top();
}};
int main() {
    Calculator calculator;
    cout << calculator.calc("2.2+50.4") << endl;
    cout << calculator.calc("2+4*5") << endl;
    cout << calculator.calc("4*(9+4)") << endl;
    cout << calculator.calc("(7+11)/2") << endl;
    cout << calculator.calc("7+0") << endl;
    cout << calculator.calc("(0-0)") << endl;
    cout << calculator.calc("0") << endl;
    cout << calculator.calc("") << endl;
    return 0;
}
