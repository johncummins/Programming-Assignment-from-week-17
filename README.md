# Programming-Assignment-from-week-17
// John Cummins; Assignment Week 17, 2.cpp : Defines the entry point for the console application.
//

#define _CRT_SECURE_NO_DEPRECATE
#include "stdafx.h"
#include <stdio.h>
#include "string.h"
#include "stdlib.h"

struct date
{
	int day, month, year;
};

struct record
{
	struct date orderDate;
	char region[20];
	char rep[20];
	char item[20];
	int units;
	float unitCost;
	float totalCost;
};

int nrecords = 0;
struct record records[501];
int readFile(char *fileName);
void findByRegion(char *region);
void findByRep(char *rep);
void findByItem(char *item);
void findByYear(int year);

int getOption();

void main()
{
	char string[20];
	char fileName[] = "\\fs2\16373731\SampleData.txt";
	int i = 0;
	int option, year;
	char temp[20];

	if (!readFile(fileName))
	{
		printf("File could not be opened !!\n");
		return;
	}
	option = getOption();


	while (option != 0)
	{
		switch (option)
		{
		case 1:
			puts("Enter Region");
			gets_s(temp);
			findByRegion(temp);
			break;
		case 2:
			puts("Enter Rep");
			gets_s(temp);
			findByRep(temp);
			break;
		case 3:
			puts("Enter Item");
			gets_s(temp);
			findByItem(temp);
			break;
		case 4:
			puts("Enter Year");
			gets_s(temp);
			year = atoi(temp);
			findByYear(year);
			break;
		}
		option = getOption();

	}
}

int readFile(char *fileName)
{

	char date[12];
	char line[200];
	FILE *fptr = fopen(fileName, "r");
	char *token;
	char delim[4] = "\t";
	int n = 0, i = 0;

	if (fptr == NULL)
	{
		printf("Error opening file ! \n");
		return 0;
	}

	fgets(line, 200, fptr);

	fgets(line, 200, fptr);
	while (!feof(fptr))
	{
		if ((token = strtok(line, delim))
		!= NULL) strcpy(date, token);
		if ((token = strtok(NULL, delim)) 
		!= NULL) strcpy(records[n].region, token);
		if ((token = strtok(NULL, delim)) 
		!= NULL) strcpy(records[n].rep, token);
		if ((token = strtok(NULL, delim)) 
		!= NULL) strcpy(records[n].item, token);
		if ((token = strtok(NULL, delim)) 
		!= NULL) records[n].units = atoi(token);
		if ((token = strtok(NULL, delim)) 
		!= NULL) records[n].unitCost = atof(token);
		if ((token = strtok(NULL, delim)) 
		!= NULL) records[n].totalCost = atof(token);

		token = strtok(date, "/");
		records[n].orderDate.day = atoi(token);
		token = strtok(NULL, "/");
		records[n].orderDate.month = atoi(token);
		token = strtok(NULL, "\t");
		records[n].orderDate.year = atoi(token);

		n++;
		fgets(line, 200, fptr);
	}

	nrecords = n;

	fclose(fptr);

	return 1;
}


void findByRegion(char *region)
{
	int i = 0;
	float total = 0.0;

	for (i = 0; i<nrecords; i++)
	{
		if (!strcmp(records[i].region, region))
		{
			total = total + records[i].totalCost;
		}
	}
	printf("Sales for region: %s = %.2f\n", region, total);
}

void findByRep(char *rep)
{

	int i = 0;
	float total = 0.0;

	for (i = 0; i <nrecords; i++)
	{
	    if (!strcmp(records[i].rep, rep))
		{
			total = total + records[i].totalCost;
		}
	}
	printf("Sales for rep: %s = %.2f\n", rep, total);
}

void findByItem(char *item)
{

	int i = 0;
	float total = 0.0;

	for (i = 0; i <nrecords; i++)
	{
		if (!strcmp(records[i].item, item))
		{
			total = total + records[i].totalCost;
		}
	}
	printf("Sales for item: %s = %.2f\n", item, total);

}

void findByYear(int year)
{

	int i = 0;
	float total = 0.0;

	for (i = 0; i <nrecords; i++)
	{
		if (records[i].orderDate.year == year)
		{
			total = total + records[i].totalCost;
		}
	}
	printf("Sales for year: %d = %.2f\n", year, total);
}


int getOption()
{
	int option = 0;
	char inp[3];

	puts("Enter one of the follow options:");
	puts("1 - sales by region");
	puts("2 - sales by rep");
	puts("3 - sales by item");
	puts("4 - sales by year");
	puts("0 - quit");
	gets_s(inp);
	option = atoi(inp);

	return option;
}
