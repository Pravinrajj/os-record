# os-record
## rr
```
#include<stdio.h>
struct process
{
  int burst;
  int completion;
  int remain;
  int turn;
  int wait;
}; 
int main()
{
  int n,a,i,count=0,currenttime=0;
  printf("Enter no. of process");
  scanf("%d",&n);
  printf("Enter quantum time");
  scanf("%d",&a);
  struct process pro[n];
  printf("Enter the burst time");
  for(i=0;i<n;i++)
  {
    
    scanf("%d",&pro[i].burst);
    pro[i].remain=pro[i].burst;
  }
  while(count<n)
  {
    for(i=0;i<n;i++)
    {
      if(pro[i].remain>0)
      {
        if(pro[i].remain<=a)
        {
          currenttime+=pro[i].remain;
          pro[i].completion=currenttime;
          pro[i].remain=0;
          count++;
	}
	else
	{
	  currenttime+=a;
	  pro[i].remain-=a;
	}
	
      }
    }
  }
  int tat=0,wat=0;
  for(i=0;i<n;i++)
  {
    pro[i].turn=pro[i].completion;
    pro[i].wait=pro[i].turn-pro[i].burst;
    tat+=pro[i].turn;
    wat+=pro[i].wait;
  }
  float atat=(float)tat/n;
  float awat=(float)wat/n;
  printf("%.2f\n",atat);
  printf("%.2f",awat);
}
```

## rr 
```
#include <stdio.h>

int main() {
    int st[10], bt[10], wt[10], tat[10], n, tq;
    int i, count = 0, swt = 0, stat = 0, temp, sq = 0;
    float awt, atat;

    printf("Enter the number of processes: ");
    scanf("%d", &n);

    printf("Enter the burst time of each process:\n");
    for (i = 0; i < n; i++) {
        printf("P%d: ", i + 1);
        scanf("%d", &bt[i]);
        st[i] = bt[i];
    }

    printf("Enter the time quantum: ");
    scanf("%d", &tq);

    while (1) {
        for (i = 0, count = 0; i < n; i++) {
            temp = tq;
            if (st[i] == 0) {
                count++;
                continue;
            }
            if (st[i] > tq)
                st[i] -= tq;
            else if (st[i] >= 0) {
                temp = st[i];
                st[i] = 0;
            }
            sq += temp;
            tat[i] = sq;
        }
        if (n == count)
            break;
    }

    for (i = 0; i < n; i++) {
        wt[i] = tat[i] - bt[i];
        swt += wt[i];
        stat += tat[i];
    }

    awt = (float)swt / n;
    atat = (float)stat / n;

    printf("Process no\t Burst time\t Waiting time\t Turnaround time\n");
    for (i = 0; i < n; i++)
        printf("%d\t\t %d\t\t %d\t\t %d\n", i + 1, bt[i], wt[i], tat[i]);

    printf("Avg waiting time=%f\nAvg turn around time=%f\n", awt, atat);

    return 0;
}

```
## pri
```
#include<stdio.h>

int main() {
    int n, i, tot1, tot2 = 0;
    float awa, atat;

    printf("Enter the no.of process: ");
    scanf("%d", &n);

    int bt[n], tat[n], wa[n], pr[n];

    printf("Enter the burst time and priority:\n");

    for (i = 0; i < n; i++) {
        printf("P%d: ", i + 1);
        scanf("%d %d", &bt[i], &pr[i]);
    }

    for (i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1; j++) {
            if (pr[j] > pr[j + 1]) {
                int temp = bt[j];
                bt[j] = bt[j + 1];
                bt[j + 1] = temp;
                temp = pr[j];
                pr[j] = pr[j + 1];
                pr[j + 1] = temp;
            }
        }
    }

    tat[0] = bt[0];
    tot1 = bt[0];

    for (i = 1; i < n; i++) {
        tat[i] = tat[i - 1] + bt[i];
        tot1 += tat[i];
    }

    atat = (float)tot1 / n;

    for (i = 0; i < n; i++) {
        wa[i] = tat[i] - bt[i];
        tot2 += wa[i];
    }

    awa = (float)tot2 / n;

    printf("Avg Turn around time : %.3f\n", atat);
    printf("Avg Waiting time : %.3f\n", awa);

    return 0;
}

```

## pri
```
#include<stdio.h>
void avg(int bt[],int n)
{
   int i,tat[n],wt[n],total_tat=0,wtt=0;
   float avg_t,avg_w;
   for(i=0;i<n;i++)
   {
     if(i==0)
         wt[0]=0;
     else
     {
       wt[i]=wt[i-1]+bt[i-1];
     }    
     tat[i]=wt[i]+bt[i];
   }
   for(i=0;i<n;i++)
   {
     total_tat+=tat[i];
     wtt+=wt[i];
     
   } 
   avg_t= (float)total_tat/n;
   avg_w=(float)wtt/n;
   printf("%f",avg_t);
   printf("%f",avg_w);
     
}
int main()
{
  int n,i,j,temp;
  printf("Enter the number of process\n");
  scanf("%d",&n);
  int bt[n],pn[n];
  for(i=0;i<n;i++)
  {
     scanf("%d %d",&bt[i],&pn[i]);
     
  }
 for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (pn[j]> pn[j+1]) {
                int temp = bt[j];
                bt[j] = bt[j+1];
                bt[j+1] = temp;
                temp = pn[j];
                pn[j] = pn[j+1];
                pn[j+1] = temp;
            }
        }
     
  }
  for(i=0;i<n;i++)
  {
     printf("%d\n",bt[i]);
  }
  avg(bt,n);
  
}
```
## ipc
```
#include<stdio.h>
#include<unistd.h>
int main()
{
    int fd[2],child;
    char a[100];
    scanf("%[^\n]",a);
    child=fork();
    if(!child)
    {
        close(fd[0]);
        write(fd[1],a,5);
    }
    else
    {
        close(fd[1]);
        read(fd[0],a,5);
        printf("The string received from the pipe is: %s",a);
    }
}
```
## syscall
```
#include <stdio.h>
#include <fcntl.h>
#include <sys/types.h>
#include <unistd.h>
#include<string.h>
int main() {
    int f1, f2,i;
    char  strin[100];
    f1 = open("data", O_WRONLY | O_CREAT | O_APPEND, 0644);
   scanf("%[^\n]",strin);
   i=strlen(strin);
    write(f1, strin, i);
    close(f1);
    f2 = open("data", O_RDONLY);
    read(f2, strin, i);
    close(f2);
    int pid = fork();
    if (pid == 0) printf("Child process: %s\n", strin);
    else if (pid > 0) printf("Parent process: %s\n", strin);
    else perror("Fork failed");
}
```
## disk_fcfs
```
#include <stdio.h>
#include <stdlib.h>
int main() {
    int RQ[100], n, TotalHeadMoment = 0, initial;
    printf("Enter the number of Requests: ");
    scanf("%d", &n);
    printf("Enter the Requests sequence:\n");
    for (int i = 0; i < n; i++)
        scanf("%d", &RQ[i]);
    printf("Enter initial head position: ");
    scanf("%d", &initial);
    for (int i = 0; i < n; i++) {
        TotalHeadMoment += abs(RQ[i] - initial);
        initial = RQ[i];
    }
    printf("Total head moment is %d\n", TotalHeadMoment);
}
```
## disk_sstf
```
#include<stdio.h>
#include<stdlib.h>
int main() {
    int RQ[100], n, TotalHeadMoment = 0, initial, count = 0;
    printf("Enter the number of Requests\n");
    scanf("%d", &n);
    printf("Enter the Requests sequence\n");
    for (int i = 0; i < n; i++)
        scanf("%d", &RQ[i]);
    printf("Enter initial head position\n");
    scanf("%d", &initial);
    while (count != n) {
        int min = 1000, d, index;
        for (int i = 0; i < n; i++) 
        {
            d = abs(RQ[i] - initial);
            if (min > d) 
            {
                min = d;
                index = i;
            }
        }
        TotalHeadMoment += min;
        initial = RQ[index];
        RQ[index] = 1000;
        count++;
    }
    printf("Total head movement is %d\n", TotalHeadMoment);
}
```
## fifo
```
#include<stdio.h>
int main()
{
    int n,count=0,avail,no,i,j,k,frame[10];
    printf("Enter the no.of pages: ");
    scanf("%d",&n);
    int a[n];
    printf("Enter the page num: ");
    for(i=0;i<n;i++)
    scanf("%d",&a[i]);
    printf("Enter the no.of frame: ");
    scanf("%d",&no);
    for(i=0;i<no;i++)
    frame[i]=-1;
    j=0;
    for(i=0;i<n;i++)
    {
        printf("%d\t\t",a[i]);
        avail=0;
        for(k=0;k<no;k++)
        {
            if(frame[k]==a[i])
            avail=1;
        }
        if(avail==0)
        {
            frame[j]=a[i];
            j=(j+1)%no;
            count++;
            for(k=0;k<no;k++)
            printf("%d ",frame[k]);
        }
        printf("\n");
    }
    printf("Page fault is: %d",count);
}
```
## LRU
```
#include<stdio.h>
int main() {
    int q[20], p[50], c = 0, i, j, k = 0, n, f;
    printf("Enter no of pages:\n");
    scanf("%d", &n);
    printf("Enter the reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &p[i]);
    printf("Enter no of frames:\n");
    scanf("%d", &f);
    q[k++] = p[0];
    printf("\t\n\t %d\n", q[0]);
    for (i = 1; i < n; i++) {
        int isPageFault = 1;
        for (j = 0; j < f; j++) {
            if (p[i] == q[j]) {
                isPageFault = 0;
                break;
            }
        }
        if (isPageFault) {
            if (k < f) {
                q[k++] = p[i];
            } else {
                for (j = 0; j < f - 1; j++) {
                    q[j] = q[j + 1];
                }
                q[f - 1] = p[i];
            }
            c++;
            for (j = 0; j < k; j++)
                printf("\t%d", q[j]);
            printf("\n");
        }
    }
    printf("\nThe number of page faults is %d\n", c);
}
```
## Seq allocation
```
#include <stdio.h>
int main() {
    int f[50],st, len, j, c;
    printf("Files Allocated are :\n");
    do {
        int count = 0;
        printf("Enter starting block and length of files: ");
        scanf("%d %d", &st, &len);
        for (int i = st; i < (st + len); i++)
            count += (f[i] == 0);
        if (len == count) 
        {
            for (j = st; j < (st + len); j++) 
            {
                if (f[j] == 0) 
                {
                    f[j] = 1;
                    printf("%d\t%d\n", j, f[j]);
                }
            }
            if (j != (st + len - 1))
                printf("The file is allocated to disk\n");
        }   
        else 
        {
            printf("The file is not allocated\n");
        }
        printf("Do you want to enter more files (Yes - 1/No - 0): ");
        scanf("%d", &c);
    } while (c == 1);
}
```
## ind allocation
```
#include<stdio.h>
#include<stdlib.h>
int main() {
    int f[50], index[50], n, ind, count, j, k, c;
    while (1) {
        printf("Enter the index block: ");
        scanf("%d", &ind);
        if (f[ind] == 1) {
            printf("%d index is already allocated \n", ind);
            continue;
        }
        printf("Enter the number of blocks needed for index %d on the disk: \n", ind);
        scanf("%d", &n);
        count = 0;
        for (int i = 0; i < n; i++) {
            scanf("%d", &index[i]);
            count += (f[index[i]] == 0);
        }
        if (count == n) 
        {
            for (j = 0; j < n; j++)
                f[index[j]] = 1;
            printf("Allocated\nFile Indexed\n");
            for (k = 0; k < n; k++)
                printf("%d ------- >%d : %d\n", ind, index[k], f[index[k]]);
        } 
        else {
            printf("File in the index is already allocated \n");
            printf("Enter another file index\n");
            continue;
        }
        printf("Do you want to enter more files (Yes - 1/No - 0): ");
        scanf("%d", &c);
        if (c != 1)
            break;
    }
}
```
## Linked allocation
```
#include <stdio.h>
#include <stdlib.h>
int main() {
    int pages[50], p, a, st, len, k, c, j;
    printf("Enter the number of blocks already allocated: ");
    scanf("%d", &p);
    printf("Enter the blocks already allocated: ");
    for (int i = 0; i < p; i++) {
        scanf("%d", &a);
        pages[a] = 1;
    }
    do {
        printf("Enter the index of the starting block and its length: ");
        scanf("%d %d", &st, &len);
        k = len;
        if (pages[st] == 0) 
        {
            for (j = st; j < (st + k); j++) 
            {
                if (pages[j] == 0) 
                {
                    pages[j] = 1;
                    printf("%d ----- >%d\n", j, pages[j]);
                } 
                else 
                {
                    printf("The block %d is already allocated \n", j);
                    k++;
                }
            }
        } 
        else {
            printf("The block %d is already allocated \n", st);
        }
        printf("Do you want to enter more files? \n");
        printf("Enter 1 for Yes, Enter 0 for No: ");
        scanf("%d", &c);
    } while (c == 1);
}
```

```
