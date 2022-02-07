# January 31, 2022: More Void* Stuff

> Midterm is on Tuesday, and it's probitably going to be a website where you fill it out.
> 
> Generics, etc. are going to be on the midterm

## Misc GDB Tips

- `step`: executes next line IN function calls

- `display var_name`: display variable after each step

- `finish`: finishes functions and leaves

- `where`: says where in program I am (important for segfault)

## qsort Functions

`typedef` allows for better struct referencing:

```c
typedef struct rect {
    int width;
    int height;
} rect; // we can refer to this struct as rect now
```

```c
int rect_comp_area(const void *r1, const void *r2) {
    const rec *r1ptr = (const rect*) r1; // don't need to cast
    const rec *r2ptr = r2;

    int area1 = r1ptr->width * r1ptr->height;
    int area2 = r2ptr->width * r2ptr->height;

    return area1 - area2;
}


int main(int argc, char** argv) {
    // ...
}
```

## More structs: Let's make a stack!

```c
typedef struct node {
    struct node *next;
    void *data; // because we don't know what type we'll have in the node
} node;


typedef struct stack {
    int elem_size_bytes;
    int nelems;
    node* top;
} stack;

stack* stack_create(int elem_size_bytes) {
    stack *s = malloc(sizeof(stack));
    s->elem_size_bytes = elem_size_bytes;
    s->nelems = 0;
    s->top = NULL;
    return s;
}

void stack_push(stack *s, void *addr) {
    node *new_node = malloc(sizeof(node));
    assert(new_node != NULL);

    // copy data
    new_node->data = malloc(s->elem_size_bytes); // double malloc
    memcpy(new_node->data, addr, s->elem_size_bytes);
    new_node->next = s->top;
    s->top = new_node;
    s->nelems++;
}

void stack_pop(stack *s, void *addr) {
    if (s->nelems == 0) 
        error(1,0,"can't pop, stack size 0");
    node *n = s->top;
    memcpy(addr, n->data, s->elem_size_bytes);

    //rewire
    s->top = n->next;
    free(n->data);
    free(n);
    s->nelems--;
}

int main(int argc, char **argv) {
    stack *intstack = stack_create(sizeof(int));
    for (int i = 0; i < TEST_STACK_SIZE; i++) {
        stack_push(intstack, &i);
    }

    int popped_int;
    while (intstack->nelems > 0) {
        stack_pop(intstack, &popped_int);
        printf("%d\n". popped_int);
    }

    int num = 42;
    stack_push(intstack, &num);


    return 0;
}
```
