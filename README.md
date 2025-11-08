# DS-8
DS code
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 100
char stack[SIZE];
int top = -1;

void push(char c) { stack[++top] = c; }
char pop() { return stack[top--]; }
char peek() { return stack[top]; }
int precedence(char c) {
    if (c == '^') return 3;
    if (c == '*' || c == '/') return 2;
    if (c == '+' || c == '-') return 1;
    return 0;
}

void reverse(char *exp) {
    int i, j;
    char temp;
    for (i = 0, j = strlen(exp) - 1; i < j; i++, j--) {
        temp = exp[i];
        exp[i] = exp[j];
        exp[j] = temp;
    }
}

int main() {
    char infix[SIZE], prefix[SIZE], temp[SIZE];
    int i, j = 0;
    printf("Enter infix expression: ");
    scanf("%s", infix);
    reverse(infix);
    for (i = 0; infix[i]; i++) {
        if (infix[i] == '(') infix[i] = ')';
        else if (infix[i] == ')') infix[i] = '(';
    }
    for (i = 0; infix[i]; i++) {
        if (isalnum(infix[i]))
            temp[j++] = infix[i];
        else if (infix[i] == '(')
            push(infix[i]);
        else if (infix[i] == ')') {
            while (top != -1 && peek() != '(')
                temp[j++] = pop();
            pop();
        } else {
            while (top != -1 && precedence(peek()) > precedence(infix[i]))
                temp[j++] = pop();
            push(infix[i]);
        }
    }
    while (top != -1)
        temp[j++] = pop();
    temp[j] = '\0';
    reverse(temp);
    printf("Prefix expression: %s\n", temp);
    return 0;
}
