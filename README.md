# dbms

#include <stdio.h>
#include <stdlib.h>
struct Node {
    int keys[3];       // Max 3 keys per node
    int count;         // Current keys count
    int leaf;          // 1=leaf, 0=internal
    struct Node* child[4];  // Child pointers
};
struct Node* createNode(int leaf) {
    struct Node* node = (struct Node*)malloc(sizeof(struct Node));
    node->count = 0;
    node->leaf = leaf;
    for(int i=0; i<4; i++) node->child[i] = NULL;
    return node;
}
void insertLeaf(struct Node* node, int key) {
    int i = node->count - 1;
    while(i >= 0 && key < node->keys[i]) {
        node->keys[i+1] = node->keys[i];
        i--;
    }
    node->keys[i+1] = key;
    node->count++;
    printf("Inserted %d\n", key);
}
void search(struct Node* node, int key) {
    int i = 0;
    while(i < node->count && key > node->keys[i]) i++;
    
    if(i < node->count && key == node->keys[i]) {
        printf("Key %d FOUND\n", key);
    }
    else if(node->leaf) {
        printf("Key %d NOT FOUND\n", key);
    }
    else {
        search(node->child[i], key);
    }
}
void display(struct Node* node) {
    if(!node) return;
    
    for(int i=0; i<node->count; i++) {
        if(!node->leaf) display(node->child[i]);
        printf("%d ", node->keys[i]);
    }
    if(!node->leaf) display(node->child[node->count]);
}
int main() {
    struct Node* root = createNode(1);  // Start as leaf
    int choice, key;
    do {
        printf("\n1. Insert\n2. Search\n3. Display\n4. Exit\n");
        printf("Choice: ");
        scanf("%d", &choice);
        switch(choice) {
            case 1:
                printf("Enter key: ");
                scanf("%d", &key);
                insertLeaf(root, key);
                break;
            case 2:
                printf("Search key: ");
                scanf("%d", &key);
                search(root, key);
                break;
            case 3:
                printf("Tree: ");
                display(root);
                printf("\n");
                break;
            case 4:
                printf("Goodbye!\n");
                break;
            default:
                printf("Invalid choice!\n");
        }
    } while(choice != 4);
    return 0;
}
