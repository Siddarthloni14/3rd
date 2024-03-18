# 3rdDevelop a menu driven Program in C for the following operations on STACK of Integers (Array 
Implementation of Stack with maximum size MAX) 
a. Push an Element on to Stack 
b. Pop an Element from Stack 
c. Demonstrate how Stack can be used to check Palindrome 
d. Demonstrate Overflow and Underflow situations on Stack 
e. Display the status of Stack 
f. Exit Support the program with appropriate functions for each of the above operation

#include <stdio.h>
#include <stdbool.h>

#define MAX 100

// Stack structure
struct Stack {
    int items[MAX];
    int top;
};

// Function prototypes
void initializeStack(struct Stack *s);
bool isFull(struct Stack *s);
bool isEmpty(struct Stack *s);
void push(struct Stack *s, int value);
int pop(struct Stack *s);
void display(struct Stack *s);
void checkPalindrome(struct Stack *s);

int main() {
    struct Stack stack;
    initializeStack(&stack);

    char choice;
    int value;

    do {
        printf("\nStack Operations Menu:\n");
        printf("a. Push an element\n");
        printf("b. Pop an element\n");
        printf("c. Check Palindrome\n");
        printf("d. Display stack status\n");
        printf("e. Exit\n");
        printf("Enter your choice: ");
        scanf(" %c", &choice);

        switch (choice) {
            case 'a':
                printf("Enter element to push: ");
                scanf("%d", &value);
                if (!isFull(&stack))
                    push(&stack, value);
                else
                    printf("Stack Overflow! Cannot push element.\n");
                break;

            case 'b':
                if (!isEmpty(&stack))
                    printf("Popped element: %d\n", pop(&stack));
                else
                    printf("Stack Underflow! Cannot pop element.\n");
                break;

            case 'c':
                checkPalindrome(&stack);
                break;

            case 'd':
                display(&stack);
                break;

            case 'e':
                printf("Exiting program.\n");
                break;

            default:
                printf("Invalid choice! Please enter a valid option.\n");
        }
    } while (choice != 'e');

    return 0;
}

// Function to initialize stack
void initializeStack(struct Stack *s) {
    s->top = -1;
}

// Function to check if the stack is full
bool isFull(struct Stack *s) {
    return s->top == MAX - 1;
}

// Function to check if the stack is empty
bool isEmpty(struct Stack *s) {
    return s->top == -1;
}

// Function to push an element onto the stack
void push(struct Stack *s, int value) {
    s->items[++s->top] = value;
}

// Function to pop an element from the stack
int pop(struct Stack *s) {
    return s->items[s->top--];
}

// Function to display the stack status
void display(struct Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty.\n");
        return;
    }

    printf("Stack elements:\n");
    for (int i = s->top; i >= 0; i--)
        printf("%d\n", s->items[i]);
}

// Function to check if the stack elements form a palindrome
void checkPalindrome(struct Stack *s) {
    if (isEmpty(s)) {
        printf("Stack is empty, cannot check palindrome.\n");
        return;
    }

    int temp[MAX];
    int i = 0;
    struct Stack tempStack;
    initializeStack(&tempStack);

    // Copy stack elements to temporary array and stack
    while (!isEmpty(s)) {
        int item = pop(s);
        temp[i++] = item;
        push(&tempStack, item);
    }

    // Compare stack elements with reversed order
    bool isPalindrome = true;
    for (int j = 0; j < i; j++) {
        if (temp[j] != tempStack.items[j]) {
            isPalindrome = false;
            break;
        }
    }

    if (isPalindrome)
        printf("The stack elements form a palindrome.\n");
    else
        printf("The stack elements do not form a palindrome.\n");

    // Restore original stack elements
    while (!isEmpty(&tempStack))
        push(s, pop(&tempStack));
}
