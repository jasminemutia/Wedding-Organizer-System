#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Wedding{
	char name[26];
	int fee;
	char location[31];
	int crew;
	char website[26];
	int height;
	Wedding *right, *left;
}*root = NULL;

int getHeight(Wedding *curr){
	if(!curr) return 0;
	else return curr->height;
}

int max(int a, int b){
	if(a > b) return a;
	else return b;
}

int getBalance(Wedding* curr){
	if(!curr) return 0;
	else getHeight(curr->left) - getHeight(curr->right);
}

Wedding *createWed(char name[], int fee, char location[], int crew, char website[]){
	Wedding *newWed = (Wedding*)malloc(sizeof(Wedding));
	strcpy(newWed->name, name);
	strcpy(newWed->location, location);
	strcpy(newWed->website, website);
	newWed->fee = fee;
	newWed->crew = crew;
	newWed->height = 1;
	newWed->right = newWed->left = NULL;
	
	return newWed;
}

Wedding *leftRotate(Wedding *curr){
	Wedding *x = curr->right; // x simpan anak kanan dari parent
	Wedding *y = x->left; // y simpan anak kiri dari x
	
	x->left = curr; // tukar anak kiri dengan parent
	curr->right = y; // tukar anak kanan dari parent dengan x
	
	curr->height = max(getHeight(curr->left), getHeight(curr->right)) + 1;
	x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
	
	return x;
}

Wedding *rightRotate(Wedding *curr){
	Wedding *x = curr->left; // x simpan anak kanan dari parent
	Wedding *y = x->right; // y simpan anak kiri dari x
	
	x->right = curr; // tukar anak kiri dengan parent
	curr->left = y; // tukar anak kanan dari parent dengan x
	
	curr->height = max(getHeight(curr->left), getHeight(curr->right)) + 1;
	x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
	
	return x;
}

Wedding *insert(Wedding *root, char name[], int fee, char location[], int crew, char website[]){
	if(!root) return createWed(name, fee, location, crew, website);
	if(strcmp(name, root->name) < 0) root->left = insert(root->left, name, fee, location, crew, website);
	else if(strcmp(name, root->name) > 0) root->right = insert(root->right, name, fee, location, crew, website);
	
	root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
	int balance = getBalance(root);
	
	if(balance > 1 && strcmp(name, root->left->name) < 0){
		return rightRotate(root);
	} else if(balance > 1 && strcmp(name, root->left->name) > 0){
		// left right
		root->left = leftRotate(root->left);
		return rightRotate(root);
	} else if(balance < -1 && strcmp(name, root->right->name) > 0){
		return leftRotate(root);
	} else if(balance < -1 && strcmp(name, root->right->name) < 0){
		// right left
		root->right = rightRotate(root->right);
		return leftRotate(root);
	}
	
	return root;
}

bool startsWith(char a[], char b[]) {
    int len = strlen(b);
    if (strncmp(a, b, len) == 0) return true;
    return false;
}

bool endsWith(char a[], char b[]) {
    int lenA = strlen(a);
    int lenB = strlen(b);
    if (strncmp(a + lenA - lenB, b, lenB) == 0) return true;
    return false;
}

bool ulrValidation(char url[]) {
	int len = strlen(url);
	if(len < 13 && len > 25) return false;
	if(strstr(url, " ") != 0) return false;
	if(startsWith(url, "www.") == false) return false;
	if(endsWith(url, ".wo.id") == false) return false;
	
	return true;
}

void addWed(){
	char n[26];
	do{
		printf("Wedding Organizer Name [3 - 25 characters]: ");
		scanf("%[^\n]", n); getchar();
	}while(strlen(n) < 3 || strlen(n) > 25);
	
	long int f;
	do{
		printf("Wedding Organizer Fee [1,000,000 - 50,000,000]: ");
		scanf("%ld", &f); getchar();
	}while(f < 1000000 || f > 50000000); 
	
	char l[31];
	do{
		printf("Wedding Organizer Location [4 - 30 characters]: ");
		scanf("%[^\n]", l); getchar();
	}while(strlen(l) < 4 || strlen(l) > 30);
	
	int c;
	do{
		printf("Wedding Organizer Total Crew [2 - 2,000]: ");
		scanf("%d", &c); getchar();
	}while(c < 2 || c > 2000);
	
	char url[26];
	while(true){
		printf("Wedding Organizer Website [13 - 25 characters]: ");
		scanf("%[^\n]", url); getchar();
		if(ulrValidation(url) == true) break;
	}
	
	char ch[5];
	printf("Confirm Insert [y / n] : "); 
	scanf("%s", ch); getchar();
	if(strcmp(ch, "y") == 0) {
		root = insert(root, n, f, l, c, url);
		puts("Insert success!\n");
		puts("Press Enter to continue..."); 
	} else if(strcmp(ch, "n") == 0) puts("Press Enter to continue...");
	
	getchar();
}

void menu(){
	puts("+=======================================+");	
	puts("|          WO Wedding Organizer         |");	
	puts("+=======================================+");
	puts("| 1. Add Available Wedding Organizer    |");
	puts("| 2. Choose Available Wedding Organizer |");
	puts("| 3. Book All Wedding Organizer         |");
	puts("| 4. View Available Wedding Organizer   |");
	puts("| 5. Exit                               |");
	puts("+=======================================+");
}

Wedding *search(Wedding *root, char n[]){
	if(!root) return NULL;
	if(strcmp(n, root->name) > 0) search(root->right, n);
	else if(strcmp(n, root->name) < 0) search(root->left, n);
	
	return root;
}

Wedding *deletion(Wedding *root, char name[]){
	if(!root) return NULL;
	else if(strcmp(name, root->name) < 0) root->left = deletion(root->left, name);
	else if(strcmp(name, root->name) > 0) root->right = deletion(root->right, name);
	else{
		if(!root->right || !root->left){
			Wedding *temp = root->left ? root->left : root->right;
			if(!temp){
				temp = root;
				root = NULL;
			}
			else *root = *temp;
			free(temp);
		}
		else{
			struct Wedding *temp = root->left;
			while(temp->right != NULL) {
				temp = temp->right;
			}
			// smua atribut copy
			strcpy(root->name, temp->name);
			strcpy(root->location, temp->location);
			strcpy(root->website, temp->website);
			root->fee = temp->fee;
			root->crew = temp->crew;
			root->left = deletion(root->left, temp->name);
		}
	}
	if(!root) return NULL;
	root->height = max(getHeight(root->left), getHeight(root->right)) + 1;
	int balance = getBalance(root);
	
	if(balance > 1 && strcmp(name, root->left->name) < 0){
		return rightRotate(root);
	} else if(balance > 1 && strcmp(name, root->left->name) > 0){
		// left right
		root->left = leftRotate(root->left);
		return rightRotate(root);
	} else if(balance < -1 && strcmp(name, root->right->name) > 0){
		return leftRotate(root);
	} else if(balance < -1 && strcmp(name, root->right->name) < 0){
		// right left
		root->right = rightRotate(root->right);
		return leftRotate(root);
	}
	
	return root;
}

Wedding *searchWed(Wedding *root){
	char na[26], ch[5];
	printf("Which Wedding Organizer that you want to choose [Name]? ");
	scanf("%[^\n]", na); getchar();
	
	if(!root){
		puts("All Wedding Organizer is fully booked...");
		return NULL;
	}
	if(search(root, na) == NULL){
		puts("Wedding Organizer doesn't exists!");
	} else if(search(root, na)){
		printf("Are you sure %s is the one for you?\n", na);
		printf("Confirm %s [y / n] : ", na); 
		scanf("%s", ch); getchar();
		if(strcmp(ch, "y") == 0){
			root = deletion(root, na);
		} else if(strcmp(ch, "n") == 0){
			puts("Okay.. we've cancelled the reservation!");
		}
	}
	puts("Press Enter to continue...");
	getchar();
}

Wedding *deleteAll(Wedding *root) {
    if (!root) return NULL;
    root->left = deleteAll(root->left);
    root->right = deleteAll(root->right);
    free(root);
    root = NULL;
    return root;
}

Wedding *preOrder(Wedding *root){
	if(!root) return NULL;
	if(root){
		printf("|%-25s|%-8ld|%-32s|%-10d|%-25s|\n", root->name, root->fee, root->location, root->crew, root->website);
		preOrder(root->left);
		preOrder(root->right);
	}
}

Wedding *inOrder(Wedding *root){
	if(!root) return NULL;
	if(root){
		inOrder(root->left);
		printf("|%-25s|%-8ld|%-32s|%-10d|%-25s|\n", root->name, root->fee, root->location, root->crew, root->website);
		inOrder(root->right);
	}
}

Wedding *postOrder(Wedding *root){
	if(!root) return NULL;
	if(root){
		postOrder(root->left);
		postOrder(root->right);
		printf("|%-25s|%-8ld|%-32s|%-10d|%-25s|\n", root->name, root->fee, root->location, root->crew, root->website);
	}
}

Wedding *reserveWed(Wedding *root){
	char c[5];
	puts("Are you going to book us all??! YAYYY!!");
	while(true){
		printf("Confirm Book All [y / n] : ");
		scanf("%s", c); getchar();	
		if(strcmp(c, "y") == 0 || strcmp(c, "n") == 0) break;
	}
	
	if(!root){
		puts("All Wedding Organizer is fully booked...");
	}
	
	if(strcmp(c, "y") == 0){
		root = deleteAll(root);
		puts("Reservation for all wedding organizer succeed!");
	}
	else if(strcmp(c, "n") == 0){
		puts("Okay... we've cancelled all the reservation!");
	}
	
	puts("Press Enter to continue..."); getchar();
}

void cls(){
	for(int i = 0; i < 30; i++) printf("\n");
}

void printWed(){
	int c;
	do{
		puts("In which way do you want to look at the data?");
		puts("1. PreOrder");
		puts("2. InOrder");
		puts("3. PostOrder");
		printf(">> ");
		scanf("%d", &c); getchar();
		puts("==========================================================================================================");
		puts("|Wedding Organizer Name   |Price   |Location                        |Total Crew|Website                  |");
		puts("==========================================================================================================");
		switch(c){
			case 1:{
				preOrder(root);
				puts("==========================================================================================================");
//				system("pause");
				puts("Press Enter to Continue..."); getchar();
				break;
			}
			case 2:{
				inOrder(root);
				puts("==========================================================================================================");
//				system("pause");
				puts("Press Enter to Continue..."); getchar();
				break;
			}
			case 3:{
				postOrder(root);
				puts("==========================================================================================================");
//				system("pause");
				puts("Press Enter to Continue..."); getchar();
				break;
			}
			case 4:{
				break;
			}
		}
	}while(c != 4);
}

int main(){
	root = insert(root, "Jeannie", 10000000, "Jakarta", 3, "www.jasmine.wo.id");
	root = insert(root, "Arnold", 12000000, "Jakarta", 5, "www.angelbaby.wo.id");
	root = insert(root, "Wo Boutique", 1000000, "Jakarta", 2, "www.raisanyanyi.wo.id");
//	root = insert(root, "Alice", 1000000, "Jakarta", 2, "www.raisanyanyi.wo.id");
	int ch;
	do{
		cls();
		menu();
		printf(">> ");
		scanf("%d", &ch); getchar();
		
		switch(ch){
			case 1:{
				addWed();
				break;
			}
			case 2:{
				puts("==========================================================================================================");
				puts("|Wedding Organizer Name   |Price   |Location                        |Total Crew|Website                  |");
				puts("==========================================================================================================");
				inOrder(root);
				searchWed(root);
				break;
			}
			case 3:{
				// remove
				reserveWed(root);
				break;
			}
			case 4:{
				// if theres still available
				printWed();
				break;
			}
			case 5:{
				root = deleteAll(root);
				break;
			}
		}
		
	}while(ch != 5);
	
	
	return 0;
}
