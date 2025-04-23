# algoritma-analizi-dev
#include <stdio.h>
#include <stdlib.h>

// İkili Arama Ağacı düğüm yapısı
struct Node {
    int data;
    struct Node* left;
    struct Node* right;
};

// Yeni bir düğüm oluşturma fonksiyonu
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Ağaca eleman ekleme fonksiyonu
struct Node* insert(struct Node* root, int data) {
    if (root == NULL) {
        return createNode(data);
    }
    if (data < root->data) {
        root->left = insert(root->left, data);
    } else if (data > root->data) {
        root->right = insert(root->right, data);
    }
    return root;
}

// Ağacı sıralı dolaşma (In-order Traversal) fonksiyonu
void inorderTraversal(struct Node* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d ", root->data);
        inorderTraversal(root->right);
    }
}

// Ağacın en küçük elemanını bulma fonksiyonu
struct Node* findMin(struct Node* root) {
    while (root->left != NULL) {
        root = root->left;
    }
    return root;
}

// Ağacın en büyük elemanını bulma fonksiyonu
struct Node* findMax(struct Node* root) {
    while (root->right != NULL) {
        root = root->right;
    }
    return root;
}

// Ağacın yüksekliğini hesaplama fonksiyonu
int calculateHeight(struct Node* root) {
    if (root == NULL) {
        return 0;
    }
    int leftHeight = calculateHeight(root->left);
    int rightHeight = calculateHeight(root->right);
    return (leftHeight > rightHeight ? leftHeight : rightHeight) + 1;
}

// Ağacın toplam düğüm sayısını hesaplama fonksiyonu
int countNodes(struct Node* root) {
    if (root == NULL) {
        return 0;
    }
    return countNodes(root->left) + countNodes(root->right) + 1;
}

// Ağacın bir düğümünü silme fonksiyonu
struct Node* deleteNode(struct Node* root, int sayi) {
    if (root == NULL) {
        return root;
    }

    if (sayi < root->data) {
        root->left = deleteNode(root->left, sayi);
    } else if (sayi > root->data) {
        root->right = deleteNode(root->right, sayi);
    } else {
        if (root->left == NULL) {
            struct Node* temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct Node* temp = root->left;
            free(root);
            return temp;
        }
        struct Node* temp = findMin(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}

// Tüm ağacı silme fonksiyonu
void deleteTree(struct Node* root) {
    if (root == NULL) {
        return;
    }
    deleteTree(root->left);
    deleteTree(root->right);
    printf("%d silindi\n", root->data);
    free(root);
}

int main() {
    struct Node* root = NULL;
    int n, value, sayi;

    // Kullanıcıdan elemanları al ve ağaca ekle
    printf("Kac sayi eklemek istiyorsun: ");
    scanf("%d", &n);

    printf("Sayilari gir:\n");
    for (int i = 0; i < n; i++) {
        scanf("%d", &value);
        root = insert(root, value);
    }

    // Ağacın son hali
    printf("\nAgacin son hali (In-order traversal): ");
    inorderTraversal(root);

    // Bir eleman sil
    printf("\nSilmek istedigin sayi: ");
    scanf("%d", &sayi);
    root = deleteNode(root, sayi);
    printf("\nAgacin son hali (Silme sonrasi): ");
    inorderTraversal(root);

    // Ağacın en küçük ve en büyük elemanlarını bul
    printf("\nEn kucuk eleman: %d", findMin(root)->data);
    printf("\nEn buyuk eleman: %d", findMax(root)->data);

    // Ağacın yüksekliğini hesapla
    printf("\nAgacin yuksekligi: %d", calculateHeight(root));

    // Ağacın toplam düğüm sayısını hesapla
    printf("\nToplam dugum sayisi: %d", countNodes(root));

    // Ağacı tamamen sil
    printf("\nTüm ağacı silmek için devam edin...\n");
    deleteTree(root);
    root = NULL; // Kökü sıfırla
    printf("Agac tamamen silindi.\n");

    return 0;
}
