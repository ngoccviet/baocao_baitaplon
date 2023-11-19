#include<stdio.h>
#include <conio.h>
#include <string.h>
#include <stdlib.h>
#define MAX 100
#include <unistd.h> //sleep()
struct user {
    char username[20];
    char password[20];
    struct user *next;
};
// Ham dang ki tai khoan
void regi(struct user **head) {
//dung con tro cap 2 de thay doi con tro dau danh sach de them nguoi dung moi vao
    struct user *newUser = (struct user*)malloc(sizeof(struct user));
    printf("Nhap ten tai khoan: ");
    scanf("%s", newUser->username);
    printf("Nhap mat khau: ");
    scanf("%s", newUser->password);
    newUser->next = NULL;
    struct user *temp = *head;
    while(temp != NULL) {
        if(strcmp(newUser->username, temp->username) == 0) {
            printf("Tai khoan da ton tai. Vui long su dung ten dang nhap khac!\n");
            free(newUser);
            return;
        }
        temp = temp->next;
    }
    //noi vao dau danh sach lk neu danh sach rong
   if(*head == NULL) {
        *head = newUser;
        printf("Dang ki thanh cong!\n");
    //noi vao duoi dslk
    } else {
        temp = *head;
        while(temp->next != NULL) {
            temp = temp->next;
        }
        temp->next = newUser;
        printf("Dang ki thanh cong!\n");
    }
}
//ham dang nhap
void login(struct user *head, int &role) {
    char username[20];
    char password[20];
    int count = 0;
	//chay sai qua 3 lan thi se cho thuc hien lai viec chon Dang ky hoac Dang nhap
    while(count < 3) {
        printf("Nhap ten tai khoan: ");
        scanf("%s", username);
        printf("Nhap mat khau: ");
        scanf("%s", password); 
		struct user *temp = head;
		//Tai khoan Admin la: ten dang nhap: admin; mk:admin
		//Tai khoan nguoi dung can phai dang ky, tai khoan admin khong can dang ki
        if(strcmp(username, "admin") == 0 && strcmp(password,"admin") == 0) {
		role = 1; 
        return;
    	}
    	else { 
        while(temp != NULL) {
            if(strcmp(username, temp->username) == 0 && strcmp(password, temp->password) == 0) {
            	system("cls"); 
                printf("DANG NHAP THANH CONG!\n");
                sleep(1); 
                printf ("Chuc ban su dung dich vu vui ve!\n");
                sleep(1);
                system("cls");
                role =2; return;
            }
            temp = temp->next;
    }
        printf("Tai khoan khong ton tai. Vui long thu lai!\n");
        count++;
    }
}
    printf("Sai qua nhieu lan. Hay thao tac lai!\n");
    role =3;
    return;
}

typedef struct Date{
	int day;
	int month;
	int year;
} date;
typedef struct Room{
	int id;
	char name[50];
	char address[50];
	char type[20]; //Single Room, Twin Room, Double Room, Triple Room, Quad Room
	int price;
	char contact[20]; //email / hotline
	char state[20];
	date start; //ngay bat dau cho thue
	date end; //ngay ket thuc thoi gian thue
}room; 
//Ham ve mot duong thang
void drawLine(int n){
	printf ("\n");
	for (int i=0;i<n;i++) printf ("_");
	printf ("\n");
}
//Ham tim ID lon nhat
int IDMAX (room a[], int n) {
    int idMax = 0;
    if (n > 0) {
        idMax = a[0].id;
        for(int i = 0;i < n; i++) {
            if (a[i].id > idMax) {
                idMax = a[i].id;
            }
        }
    }
    return idMax;
}
//Ham in danh sach phong ra man hinh
void showRoom(room a[], int n) {
    drawLine(100);
    printf ("%-5s%-5s%-16s%-15s%-15s%-16s%-20s%-20s%-15s%-15s","STT","ID","Ten Khach san","Dia chi","Loai Phong","Gia phong/dem","Lien he dat phong","Trang thai phong","Ngay bat dau","Ngay ket thuc");
    for(int i = 0; i < n; i++) {
        // in phong thu i ra man hinh
        printf ("\n");
        printf("%-5d", i + 1);
        printf("%-5d", a[i].id);
        printf("%-16s", a[i].name);
        printf("%-15s", a[i].address);
        printf("%-15s", a[i].type);
        printf("%-16d", a[i].price);
    	printf("%-20s", a[i].contact);
    	printf("%-20s", a[i].state);
        printf("%d/%d/%-12d", a[i].start.day, a[i].start.month, a[i].start.year);
		printf("%d/%d/%d", a[i].end.day, a[i].end.month, a[i].end.year);
    }
    drawLine(100);
}
void roomType(){
	printf ("\n( 1-Single Room \t2-Twin Room \t3-Double Room \t4-Triple Room \t5-Quad Room )\n");
}
//Ham nhap thong tin phong
void roomInforInput(room &newRoom,int id){
	newRoom.id=id;
	printf ("Nhap ten Khach san : ");  fflush(stdin); gets (newRoom.name);
	printf ("Nhap dia chi phong : "); gets(newRoom.address);roomType();
	printf ("Nhap loai phong : ");fflush(stdin); gets(newRoom.type); 
	printf ("Nhap gia cho phong / dem (nghin VND): "); scanf ("%d",&newRoom.price);
	printf ("Nhap email/sdt : "); fflush(stdin);gets(newRoom.contact);
	strcpy(newRoom.state,"Chua dat");//trang thai phong
	newRoom.start.day = 0; newRoom.start.month = 0; newRoom.start.year = 0;
	newRoom.end.day=0; newRoom.end.month=0; newRoom.end.year=0;
}
//Ham nhap phong
void inputRoom(room a[], int id, int n) {
    drawLine(60);
    printf("\n Nhap phong: \n");
    roomInforInput(a[n], id);
    drawLine(60);
}
//Ham xoa phong
int deleteRoom(room a[], int id, int n){
	int found = 0;
    for(int i = 0; i < n; i++) {
        if (a[i].id == id) {
            found = 1;
            drawLine(60);
            for (int j = i; j < n; j++) {
                a[j] = a[j+1];
            }
            printf ("\n Da xoa phong co ID = %d",id);
            drawLine(60);
            break;
        }
    }
    if (found == 0) {
        printf("\n Phong co ID = %d khong ton tai.", id);
        return 0;
    } else {
        return 1;
    }
}
//Ham kiem tra xem thang nhap vao co bao nhieu ngay, tra ve so ngay
int checkDay(room bookRoom,int month){
	int temp;
	if (bookRoom.start.month==1 || bookRoom.start.month==3 || bookRoom.start.month==5 ||bookRoom.start.month==7 ||bookRoom.start.month==8 || bookRoom.start.month==10 ||bookRoom.start.month==12)
	{
		temp=31;
	}
	else if (bookRoom.start.month==2){
		if (bookRoom.start.year % 4 ==0) temp = 29;
		else temp=28; 
	}
	else temp =30;
	return temp;
}
//Ham cap nhat phong (Dung trong chuc nang Dat phong) 
void updateRoom(room &newRoom) {
	//tinh so ngay trong thang
	int temp;
	strcpy(newRoom.state,"Da dat");
	do {
	printf ("Nhap ngay bat dau thue (dd/mm/yy) : \n"); 
	scanf("%d%d%d",&newRoom.start.day,&newRoom.start.month,&newRoom.start.year);
	temp=checkDay(newRoom,newRoom.start.month);
	if ( newRoom.start.day<1||newRoom.start.day>temp || newRoom.start.month<1|| newRoom.start.month >12 || newRoom.start.year<23|| newRoom.start.year >99) printf ("\nLoi! Hay nhap ngay co that! \n");}
	while (newRoom.start.day<1||newRoom.start.day>temp || newRoom.start.month<1|| newRoom.start.month >12 || newRoom.start.year<23|| newRoom.start.year >99);

	do{
		printf ("Nhap ngay ket thuc (dd/mm/yy) : \n"); 
		scanf("%d%d%d",&newRoom.end.day,&newRoom.end.month,&newRoom.end.year);
		temp=checkDay(newRoom,newRoom.end.month);
		if ( newRoom.end.month<1 || newRoom.end.month>12 || newRoom.end.day<1 || newRoom.end.day>temp || newRoom.end.year <newRoom.start.year || newRoom.end.year>99) printf ("\nLoi! Hay nhap ngay co that! \n");
	} while (newRoom.end.month<1 || newRoom.end.month>12 || newRoom.end.day<1 || newRoom.end.day>temp || newRoom.end.year <newRoom.start.year || newRoom.end.year>99);
}
//Ham reset lai trang thai phong, ngay thang dat phong.
void resetRoom(room &newRoom){
	strcpy(newRoom.state,"Chua dat");
	newRoom.start.day = 0; newRoom.start.month = 0; newRoom.start.year = 0;
	newRoom.end.day=0; newRoom.end.month=0; newRoom.end.year=0;
}
//Dat phong
void booking(room a[], int id, int n) {
    int found = 0;
    for(int i = 0; i < n; i++) {
        if (a[i].id == id) {
            found = 1;
            drawLine(60);
            printf ("\n Ban muon dat phong co ID = %d",id);
            updateRoom(a[i]);
            printf ("\nBan da dat phong thanh cong!");
            printf ("\nVao lich su dat phong de xem chi tiet!");
            drawLine(60);
            break;
        }
    }
    if (found == 0) {
        printf("\n Phong co ID = %d khong ton tai.", id);
    }
}
//Ham huy dat phong.
void unBooking(room a[],int id,int n){
	int found = 0;
    for(int i = 0; i < n; i++) {
        if (a[i].id == id) {
            found = 1;
            drawLine(60);
            printf ("\n Ban muon huy dat phong co ID = %d",id);
            //dua ham Reset vao.
            resetRoom(a[i]);
            printf ("\nBan da huy dat phong thanh cong!");
            drawLine(60);
            break;
        }
    }
    if (found == 0) {
        printf("\n Phong co ID = %d chua duoc dat. Ban hay xem lai ID phong!", id);
    }
}
//Ham tim phong theo ten
void nameFind(room a[], char ten[], int n) {
    room arrayFound[MAX];
    char tenPhong[30];
    int found = 0;
    for(int i = 0; i < n; i++) {
        strcpy(tenPhong, a[i].name);
        if(strstr(strupr(tenPhong), strupr(ten))) {
            arrayFound[found] = a[i];
            found++;
        }
    }
    showRoom(arrayFound, found);
}

//Ham tim phong theo dia chi 
void desFind(room a[], char ten[], int n) {
    room arrayFound[MAX];
    char tenPhong[30];
    int found = 0;
    for(int i = 0; i < n; i++) {
        strcpy(tenPhong, a[i].address);
        if(strstr(strupr(tenPhong), strupr(ten))) {
            arrayFound[found] = a[i];
            found++;
        }
    }
    showRoom(arrayFound, found);
}
//Hien thi so tien phai tra khi khach dat phong
void showPrice(room bookRoom){	
	int dayCount,daySum;
	daySum=bookRoom.end.day - bookRoom.start.day;
	while (bookRoom.end.month != bookRoom.start.month){
		dayCount = checkDay( bookRoom,bookRoom.start.month );
		daySum += dayCount;
		bookRoom.start.month ++;
	}
	//Tinh tien
	printf ("\nTong so tien can tra cho %d ngay thue cua phong co ID %d la: %d 000 VND\n",daySum,bookRoom.id,daySum*bookRoom.price); 
	printf ("Chuyen tien vao STK 049494949 MB Bank trong vong 12h ke tu khi dat phong!\n");
}
//Lich su dat phong
void bookingHistory(room a[], int n) {
    room arrayFound[MAX];
    int found = 0;
    for(int i = 0; i < n; i++) {
        if(strstr(a[i].state, "Da dat")) {
           showPrice(a[i]); 
		   arrayFound[found] = a[i];
            found++; 	
        }
    }
    printf ("\n\tDanh sach cac phong da dat :\n");
    showRoom(arrayFound, found);
}
//Ham cac phong chua duoc dat 
void notBookingHistory(room a[], int n) {
    room arrayFound[MAX];
    int found = 0;
    for(int i = 0; i < n; i++) {
        if(strstr(a[i].state, "Chua dat")) {
            arrayFound[found] = a[i];
            found++;
        }
    }
    printf ("\n\tDanh sach cac phong chua da dat :\n");
    showRoom(arrayFound, found);
}
//Ham doc file
void readFile(room a[], char fileName[], int &n){
	FILE * fp;
    fp = fopen (fileName, "rb");
    fread(&n, sizeof(n), 1, fp);
	for(int i=0; i<n; i++){
		fread(&a[i], sizeof(room), 1, fp);
		}
    fclose (fp);
}
//Ham luu du lieu vao file
void appendFile(room a[], int n, char fileName[]) {
    FILE * fp;
    fp = fopen (fileName,"wb");
    fwrite(&n, sizeof(n), 1, fp);
	for(int i=0; i<n; i++){
		fwrite(&a[i], sizeof(room), 1, fp);
	}
	fprintf(fp, "\n");
    fclose (fp);
}
void pressAnyKey() {
    printf ("\n\nBam phim bat ky de tiep tuc...");
    getch();
    system("cls");
}
void menu(){
	char fileName[] = "HotelRoomList.bin";
    room arrayRoom[MAX];
    int roomTotal;
    int idCount = 0;
    int role;
    readFile(arrayRoom,fileName,roomTotal);
    idCount = IDMAX (arrayRoom, roomTotal);
    struct user *head = NULL; 
    int mainChoice;
    Mall:
    do {
        printf("\n1. Dang ky\n");
        printf("2. Dang nhap\n");
        printf("3. Thoat\n");
        printf("Nhap lua chon: ");
        scanf("%d", &mainChoice);

        switch(mainChoice) {
            case 1:
                regi(&head);
                break;
            case 2:
				login(head,role);
            	if (role==1) goto adminApp;
                else if (role ==2) goto normalUser;
                else break;
               
            case 3:
                printf("Ban da thoat ung dung!\n");
                break;
            default:
                printf("Khong co chuc nang nay. Vui long chon lai.\n");
        }
    } while(mainChoice != 3);
	adminApp: 
	int choice; 
	do { 
        printf ("               QUAN LY UNG DUNG - ADMIN \n");
        printf ("*************************MENU**************************\n");
        printf ("**  1. Dang phong                                    **\n");
        printf ("**  2. Xoa phong                                     **\n");
        printf ("**  3. Xem danh sach phong hien co                   **\n");
        printf ("**  4. Tim phong theo ten Khach San                  **\n");
        printf ("**  5. Tim phong theo dia chi muon den               **\n");
        printf ("**  6. Xem cac phong da duoc dat                     **\n");
        printf ("**  7. Xem cac phong con trong                       **\n");
        printf ("**  8. Dang xuat                                     **\n");
        printf ("**  0. Thoat ung dung                                **\n");
        printf ("*******************************************************\n");
        printf ("Nhap tuy chon: ");
        scanf ("%d",&choice);
        system("cls");
        switch(choice){
            case 1:
                printf ("\n1.Dang phong.");
                idCount++;
                inputRoom(arrayRoom, idCount, roomTotal);
                printf("\nThem phong thanh cong!");roomTotal++;
                appendFile(arrayRoom, roomTotal, fileName);
                pressAnyKey();
                break;
            case 2:
                if(roomTotal > 0) {
                    int id;
                    printf ("\n2. Xoa phong.");
                    printf ("\n Nhap ID phong muon xoa: "); scanf ("%d",&id);
                    if (deleteRoom(arrayRoom, id, roomTotal) == 1) {
                        printf("\nPhong nay da bi xoa da bi xoa.");
                        roomTotal--;
                    }
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                appendFile(arrayRoom, roomTotal, fileName);
                pressAnyKey();
                break;
            case 3:
                if(roomTotal > 0){
                    printf ("\n3.Danh sach cac phong dang co tren ung dung.");
                    showRoom(arrayRoom, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 4:
               if(roomTotal > 0) {
                    printf ("\n4. Tim phong theo ten Khach San.");
                    char strTen[30];
                    printf ("\nNhap ten Khach San ban muon tim: ");fflush(stdin); gets(strTen);
                    nameFind(arrayRoom, strTen, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 5:
               if(roomTotal > 0) {
                    printf ("\n5. Tim phong theo dia chi muon den.");
                    char strTen[30];
                    printf ("\nNhap dia chi ban muon den: "); fflush(stdin); gets(strTen);
                    desFind(arrayRoom, strTen, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 6:
                if(roomTotal > 0){
                printf ("\n6.Xem cac phong da duoc dat :\n");
                bookingHistory(arrayRoom, roomTotal);
            }else{
                printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 7:
            	if(roomTotal > 0){
                printf ("\n7.Xem cac phong con trong :\n");
                notBookingHistory(arrayRoom, roomTotal);
            }else{
                printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 8:
            	printf ("BAN DA DANG XUAT!");
            	sleep(1);
            	system("cls");
            	goto Mall;
            	break;
            case 0:
                printf ("\nBan da chon Thoat ung dung !");
                getch();
                goto exit;
            default:
                printf ("\nKhong co chuc nang nay!");
                printf ("\nHay chon chuc nang trong hop menu.");
                pressAnyKey();
                break;
        }
        }
        while (choice !=0);
        
	normalUser:
	do { 
        printf ("                 TIM PHONG KHACH SAN \n");
        printf ("*************************MENU**************************\n");
        printf ("**  1. Danh sach cac phong dang co tren ung dung     **\n");
        printf ("**  2. Tim phong theo ten Khach San                  **\n");
        printf ("**  3. Tim phong theo dia chi muon den               **\n");
        printf ("**  4. Dat phong                                     **\n");
        printf ("**  5. Huy dat phong                                 **\n");
        printf ("**  6. Xem lich su dat phong                         **\n");
        printf ("**  7. Dang xuat                                     **\n");
        printf ("**  0. Thoat ung dung                                **\n");
        printf ("*******************************************************\n");
        printf ("Nhap tuy chon: ");
        scanf ("%d",&choice);
        switch(choice){
            case 1:
                if(roomTotal > 0){
                    printf ("\n1.Danh sach cac phong dang co tren ung dung.");
                    showRoom(arrayRoom, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 2:
                if(roomTotal > 0) {
                    printf ("\n2. Tim phong theo ten Khach San.");
                    char strTen[30];
                    printf ("\nNhap ten Khach San ban muon tim: ");fflush(stdin); gets(strTen);
                    nameFind(arrayRoom, strTen, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 3:
                if(roomTotal > 0) {
                    printf ("\n3. Tim phong theo dia chi muon den.");
                    char strTen[30];
                    printf ("\nNhap dia chi ban muon den: "); fflush(stdin); gets(strTen);
                    desFind(arrayRoom, strTen, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 4:
                if(roomTotal > 0) {
                    int id;
                    printf ("\n4. Dat phong.");
                    printf ("\n Nhap ID phong ban muon dat: "); scanf ("%d",&id);
                    booking(arrayRoom, id, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                appendFile(arrayRoom, roomTotal, fileName);
                pressAnyKey();
                break;
            
            case 5:
                if(roomTotal > 0) {
                    int id;
                    printf ("\n5. Huy dat phong.");
                    printf ("\n Nhap ID phong ban muon huy: "); scanf ("%d",&id);
                    unBooking(arrayRoom, id, roomTotal);
                }else{
                    printf ("\nDanh sach phong trong!");
                }
                appendFile(arrayRoom, roomTotal, fileName);
                pressAnyKey();
                break;
            
            case 6:
            	if(roomTotal > 0){
                printf ("\n6.Xem lich su dat phong :\n");
                bookingHistory(arrayRoom, roomTotal);
            }else{
                printf ("\nDanh sach phong trong!");
                }
                pressAnyKey();
                break;
            case 7:
            	printf ("BAN DA DANG XUAT!");
            	sleep(1);
            	system("cls");
            	goto Mall;
            case 0:
                printf ("\nBan da chon Thoat ung dung!");
                getch();
                goto exit;
            default:
                printf ("\nKhong co chuc nang nay!");
                printf ("\nHay chon chuc nang trong hop menu.");
                pressAnyKey();
                exit:
                break;
        }
        }
        while (choice != 0);
    //Giai phong bo nho .
	struct user *temp = head;
    while(temp != NULL) {
    struct user *current = temp;
    temp = temp->next;
    free(current);
    }
	appendFile(arrayRoom, roomTotal, fileName);
}
int main(){
    menu();
}
