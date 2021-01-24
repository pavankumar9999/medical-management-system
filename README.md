# medical-management-system
//Code

#include<stdio.h>
#include<stdlib.h>
#include<windows.h>
#include<conio.h>
#include<string.h>
#include<dos.h>

struct medicine{
	int id;
	char med[45];
	int price;
}m[100],*x=m;


//To view medicine list
medlis()
{
    system("cls");
	int i=0,c;
	FILE *f=fopen("MEDREC.txt","r");char t;fflush(stdin);
	printf("Medicine id\tMedicine name\t   Medicine price\n\n");
    do
	{
	fread((x+i),sizeof(struct medicine),1,f);
	printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);	++i;
	}while(!feof(f));
	fclose(f);fflush(stdin);printf("\n\nPress any key...");getche();
	main1();
}

//Add medicine to records
addmed()
{   
    system("cls");
	FILE *f=fopen("MEDREC.txt","a");int t,i=0;
	fflush(stdin);
	printf("\nEnter the medicine name: ");
	gets(m[i].med);
	printf("\nEnter Medicine id: ");
	scanf("%d",&m[i].id);
	printf("\nEnter Medicine price: ");
	scanf("%d",&m[i].price);
	fwrite (x,sizeof(struct medicine),1,f);
	printf("\nYour Medicine added Succesfully!\n");
	printf("Press any key...");getche();
	fclose(f);
	main1();
}

//medicine record upgradation
medupd()
{
	system("cls");int n,i,h=0;
	FILE *f=fopen("MEDREC.txt","r+");
	printf("Enter the Medicine id to be updated: ");
	scanf("%d",&n);
	for(i=0;i<100 && h==0;i++)
	{
		fread((x+i),sizeof(struct medicine),1,f);	
		if(m[i].id==n)
		{
		system("cls");
		printf("Medicine id\tMedicine name\t   Medicine price\n");
		printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);
		 printf("Enter update id: ");
		 scanf("%d",&m[i].id);
		 printf("Enter update medicine name: ");
		 scanf("%s",m[i].med);
		 printf("Enter update price: ");
		 scanf("%d",&m[i].price);
		 fseek(f,-(long)(sizeof(m[i])),SEEK_CUR);
		 fwrite((x+i),sizeof(struct medicine),1,f);h++;
		}
	}rewind(f);
	printf("Do you want to update another record?\tYES-->1 NO-->0\ninput: ");
	scanf("%d",&i);
	if(i==1)
	{
		medupd();
	}
	else
	{
	 fclose(f);
	 main1();
    }
	medlis();	 
}

//Search a medicine
medser()
{
	system("cls");
	printf("\nInitiate by Medicine id-->0\nInitiate by Medicine name-->1\ninput: ");int i;scanf("%d",&i);
	if(i==0)
	{
		medserid();
	}
	else medsernm();
}

//Search a medicine by id
medserid()
{
    system("cls");
	printf("Enter the Medicine id: ");
	int i=0,t;
	scanf("%d",&t);
	FILE *f=fopen("MEDREC.txt","r");
	fflush(stdin);system("cls");
	printf("Medicine id\tMedicine name\t   Medicine price\n\n");
    for(i=0;i<100;i++)
	{
	fread((x+i),sizeof(struct medicine),1,f);
	if(m[i].id==t)
	{
	printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);	
	}
	}printf("\nPress any key...");getche();fclose(f);
	main1();
}

//Serach by medicine name
medsernm()
{
    system("cls");
	printf("Enter the Medicine name: ");
	int i=0;char t[45];
	scanf("%s",t);
	FILE *f=fopen("MEDREC.txt","r");
	fflush(stdin);system("cls");
	printf("Medicine id\tMedicine name\t   Medicine price\n");
    for(i=0;i<100;i++)
	{
	fread((x+i),sizeof(struct medicine),1,f);
	if(stricmp(m[i].med,t)==0)
	{
	printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);	
	}
	}printf("\nPress any key...");getche();fclose(f);
	main1();
}

//Billing_medicine
medbill()
{
	system("cls");
    char o[85];int i,t[50],x[50],n,d,k,r,s,z=0,q=0;
	printf("\nEnter the name of customer: ");scanf("%s",o);printf("\n\n");
	printf("Enter number of medicines required: ");scanf("%d",&n);
	system("cls");
	for(i=0;i<n;i++)
	{
	printf("Enter the %d Medicine id: ",i+1);
	scanf("%d",&t[i]);	
	printf("Enter the quantity of %d : ",i+1);
	scanf("%d",&x[i]);printf("\n\n");
    }
	i=0;k=0;system("cls");
	FILE *f=fopen("MEDREC.txt","r");
	printf("Bill for Mr/Ms. %s  ---------\n\n",o);
	printf("Medicine id\tMedicine name\t   Medicine price\tQuantity\n\n");
	FILE *g=fopen("Bill.txt","a");
	fprintf(g,"Bill for Mr/Ms. %s  ---------\n\nMedicine id\tMedicine name\t   Medicine price\tQuantity\n\n",o);fclose(g);
   	while(k<n)
   	{
	r=x[k];s=t[k];
	z=buyloop(s,r,z);
	q=q+z;
	k++;
    }
    g=fopen("Bill.txt","a");
	printf("Total amount paid (paid or to be paid) is: %d\n===============================================================================\n",q);
	fprintf(g,"Total amount paid (paid or to be paid) is: %d\n===============================================================================\n\n",q);  
    fclose(g);fflush(stdin);
	printf("\n\nMr/Ms. %s do you want to purchase more medicines for anyone?\tYES-->1 NO-->0\ninput: ",o);
	scanf("%d",&d);
	if(d==1)
	{ medbill();}	
	printf("\nPress any key...");
	main1();
}

//Sub Medicine Buy loop
buyloop(int u,int v,int h)
{
	int i=0;
	FILE *f=fopen("MEDREC.txt","r");
    for(i=0;i<100;i++)
	{FILE *g=fopen("Bill.txt","a");
	fread((x+i),sizeof(struct medicine),1,f);
	if(m[i].id==u)
	{
	printf("%6d\t\t%10s\t\t%9d\t\t%d\n\n",m[i].id,m[i].med,m[i].price,v);
	fprintf(g,"%6d\t\t%10s\t\t%9d\t\t%d\n\n",m[i].id,m[i].med,m[i].price,v);
	h=(m[i].price*v);
	}fclose(g);
}return h;
}


//medicines_sold
medsol()
{
    system("cls");fflush(stdin);
	int i;FILE *g=fopen("Bill.txt","r");rewind(g);
	char c;
	while((c=fgetc(g))!=EOF)
	{
		printf("%c",c);
	}
	fclose(g);
	printf("\nPress any key...");
	getche();
	main1();
}


//Delete_a_record
meddel()
{
	system("cls");system("cd");int n,i;
	FILE *f=fopen("MEDREC.txt","r");
	FILE *s=fopen("MEDRECU.txt","w");
	printf("\n\nEnter the id of medicine to be deleted: ");
	scanf("%d",&n);
    while(!feof(f))
	{ i=0;
	     fread((x+i),sizeof(struct medicine),1,f);	
		if(m[i].id!=n)
		{
		 fwrite((x+i),sizeof(struct medicine),1,s);
		}
		i++;
	}
	fclose(s);
	fclose(f);
	rewind(f);
	rewind(s);
	fflush(stdin);
	remove("MEDREC.txt");printf("Press any key to conform.......");getche();
	rename("MEDRECU.txt","MEDREC.txt");
	printf("\n\nRecord deleted successfully! Kindly run the program again!\n\nPress any key.....");getche();exite();exit(0);
}

//User_Medicine_List
smedlis()
{
    system("cls");
	int i=0,c;
	FILE *f=fopen("MEDREC.txt","r");char t;fflush(stdin);
	printf("Medicine id\tMedicine name\t   Medicine price\n\n");
    do
	{
	fread((x+i),sizeof(struct medicine),1,f);
	printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);	++i;
	}while(!feof(f));
	fclose(f);fflush(stdin);printf("\nPress any key...");getche();
	std();
}

//User_Medicine_search
smedser()
{
	printf("\nInitiate by Medicine id-->0\nInitiate by Medicine name-->1\ninput: ");int i;scanf("%d",&i);
	if(i==0)
	{
		smedserid();
	}
	else smedsernm();
}


//USer_Search a medicine by id
smedserid()
{
    system("cls");
	printf("Enter the Medicine id: ");
	int i=0,t;
	scanf("%d",&t);
	FILE *f=fopen("MEDREC.txt","r");
	fflush(stdin);system("cls");
	printf("Medicine id\tMedicine name\t   Medicine price\n\n");
    for(i=0;i<100;i++)
	{
	fread((x+i),sizeof(struct medicine),1,f);
	if(m[i].id==t)
	{
	printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);	
	}
	}printf("\nPress any key...");getche();fclose(f);
	std();
}

//User_Serach by medicine name
smedsernm()
{
    system("cls");
	printf("Enter the Medicine name: ");
	int i=0;char t[45];
	scanf("%s",t);
	FILE *f=fopen("MEDREC.txt","r");
	fflush(stdin);system("cls");
	printf("Medicine id\tMedicine name\t   Medicine price\n");
    for(i=0;i<100;i++)
	{
	fread((x+i),sizeof(struct medicine),1,f);
	if(stricmp(m[i].med,t)==0)
	{
	printf("%6d\t\t%10s\t\t%9d\n",m[i].id,m[i].med,m[i].price);	
	}
	}printf("\nPress any key...");getche();fclose(f);
	std();
}

//User_Medicine_Bill
smedbill()
{
	system("cls");
    char o[85];int i,t[50],x[50],n,d,k,r,s,z=0,q=0;
	printf("\nEnter the name of customer: ");scanf("%s",o);printf("\n\n");
	printf("Enter number of medicines required: ");scanf("%d",&n);
	system("cls");
	for(i=0;i<n;i++)
	{
	printf("Enter the %d Medicine id: ",i+1);
	scanf("%d",&t[i]);	
	printf("Enter the quantity of %d : ",i+1);
	scanf("%d",&x[i]);printf("\n\n");
    }
	i=0;k=0;system("cls");
	FILE *f=fopen("MEDREC.txt","r");
	printf("Bill for Mr/Ms. %s  ---------\n\n",o);
	printf("Medicine id\tMedicine name\t   Medicine price\tQuantity\n\n");
	FILE *g=fopen("Bill.txt","a");
	fprintf(g,"Bill for Mr/Ms. %s  ---------\n\nMedicine id\tMedicine name\t   Medicine price\tQuantity\n\n",o);fclose(g);
   	while(k<n)
   	{
	r=x[k];s=t[k];
	z=sbuyloop(s,r,z);
	q=q+z;
	k++;
    }
    g=fopen("Bill.txt","a");
	printf("Total amount paid (paid or to be paid) is: %d\n===============================================================================\n",q);
	fprintf(g,"Total amount paid (paid or to be paid) is: %d\n===============================================================================\n\n",q);  
    fclose(g);fflush(stdin);
	printf("\n\nMr/Ms. %s do you want to purchase more medicines for anyone?\tYES-->1 NO-->0\ninput: ",o);
	scanf("%d",&d);
	if(d==1)
	{ medbill();}	
	printf("\nPress any key...");
	std();
}

//User sub billing loop
sbuyloop(int u,int v,int h)
{
	int i=0;
	FILE *f=fopen("MEDREC.txt","r");
    for(i=0;i<100;i++)
	{FILE *g=fopen("Bill.txt","a");
	fread((x+i),sizeof(struct medicine),1,f);
	if(m[i].id==u)
	{
	printf("%6d\t\t%10s\t\t%9d\t\t%d\n\n",m[i].id,m[i].med,m[i].price,v);
	fprintf(g,"%6d\t\t%10s\t\t%9d\t\t%d\n\n",m[i].id,m[i].med,m[i].price,v);
	h=(m[i].price*v);
	}fclose(g);
}return h;
}


//password
pass()
{
	printf("\n\nEnter password to sig-in as administrator\npass: ");
	int i;char c,w,p[20],r[20]="richa mam";
	for(i=0;i<9;i++)
	{
		w=getch();
		p[i]=w;
		w='*';printf("%c",w);
	}p[i]='\0';system("cls");
	if(stricmp(p,r)==0)	
	{main1();}
	else
    {
   	printf("Wrong Password!!\n\nYou will be redirected..... Press any key.....");getche();
	exite();
    }	
}

//exit
exite()
{
	system("cls");
	printf("\n\nThankyou for using! \nRegards-------> Pavan Kumar\nSee Ya! Bye!Bye!\nExiting in 1 second....\n\n");
	sleep(1);exit(0);
}

//student_access
std()
{
	system("cls");
    //Head_file
	printf("\t\t\t\t######################################\t\t\t\n");
	printf("\t\t\t\t|| Medical Record Management System ||\t\t\t\n");
	printf("\t\t\t\t######################################\t\t\t\n");
	printf("____________________________________________________________________________________________________________________\n");
	printf("\t\t\t\t\t  ---User Mode---\n");
    printf("\n\nEnter the number to choose the option: \n\n");int i;
	printf("1) Medicine List\n2) Medicine Search\n3) Buy Medicine\n4) Get Administrator acess\n5) Exit\n\ninput: ");
    scanf("%d",&i);
     switch(i)
    {
    	case 1:
    		smedlis();break;
    	case 2:
    		smedser();break;
    	case 3:
    		smedbill();break;
    	case 4:
    		pass();break;
    	case 5:
    		exite();break;
    	default:
    	  {printf("Input doesn't relate to any option. Sorry! Exiting in 1 second......");sleep(1);}    
	}
}

//Administrator access
main1()
{
	system("cls");
	//Head_file
	printf("\t\t\t\t######################################\t\t\t\n");
	printf("\t\t\t\t|| Medical Record Management System ||\t\t\t\n");
	printf("\t\t\t\t######################################\t\t\t\n");
	printf("____________________________________________________________________________________________________________________\n");
	printf("\t\t\t\t\t---Administrator Mode---\n");
	printf("\n\nEnter the number to choose the option: \n\n");int i;
	printf("1) Medicine List\n2) Add Medicine\n3) Update Medicine\n4) Medicine Search\n5) Buy Medicine\n6) Medicine sold\n7) Remove Medicine record\n8) Work like user\n9) Exit\n\ninput:  ");
    scanf("%d",&i);
    switch(i)
    {
    	case 1:
    		medlis();break;
    	case 2:
    		addmed();break;
    	case 3:
    		medupd();break;
    	case 4:
    		medser();break;
    	case 5:
    		medbill();break;
    	case 6:
    		medsol();break;
    	case 7:
    		meddel();exit(0);break;
    	case 8:
    		std();break;
    	case 9:
    		exite();break;
    	default:
    	  {printf("Input doesn't relate to any option. Sorry! Exiting in 1 second.....");sleep(1);}
	}
}

//MAIN------
main()
{  
    system("cls");system("cd");
    //Head_file
    printf("\n");
	printf("\t\t\t\t######################################\t\t\t\n");
	printf("\t\t\t\t|| Medical Record Management System ||\t\t\t\n");
	printf("\t\t\t\t######################################\t\t\t\n");
	printf("____________________________________________________________________________________________________________________");
	int i;printf("\n\nFor Administrator-->1\nFor User-->Any digit (2-9) \n\ninput:  ");scanf("%d",&i);
	if(i==1)
	{
   pass();
    }
   else
    {
	std();
    }
}

