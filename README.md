# Starve-free-Readers-Writers
## Problem Statement 
To implement a starvation free solution for the classic Reader-Writers Problem.
## Pseudocode Code Solution
semaphore mutex; // Binary Semaphore and initialized to 1  
semaphore rs;    // Binary Semaphore and initialized to 0  
semaphore ws;    // Binary Semaphore and initialized to 0  
semaphore wr; // Counting Semaphore and initialized to 0(number of waiting readers)  
semaphore ar;  // Counting Semaphore and initialized to 0(number of active readers)  
semaphore ww; // Counting Semaphore and initialized to 0(number of waiting writers)  
semaphore aw;  // Counting Semaphore and initialized to 0(number of active writers)  
  
Reader(){  
wait(mutex);  
if(ww+aw==0){  // If there are no writers either active or waiting  
  signal(ar);     // Since there are no writers reader will become active  
  signal(rs);     // a semaphore that locks the reader from reading if rs=0  
}  
else{  
signal(wr);   // Since writers are present wait in line  
}  
signal(mutex);  
  
wait(rs);     // if it is active reader then it will pass through else it will not  
  
  <CRITICAL SECTION>   // READING....  
  
wait(mutex);  
wait(ar); // reading is done so decrement  
  
if(ar==0&&ww>0){ // All readers are done and writers are waiting  
   while (ww>0)  // Allowing all waiting writers to enter  
        {  
           signal(ws);  // wake a writer;  
           signal(aw);  //  increment to active writers  
           wait(ww); // decrement to waiting writer  
       }  
}  
signal(mutex);  
}  

Writer(){
wait(mutex);
if(wr+ar==0){  // If there are no readers either active or waiting
  signal(aw);     // Since there are no readers writer will become active
  signal(ws);     // a semaphore that locks the writer from writing if ws=0 
}
else{
signal(ww);   // Since readers are present wait in line
}
signal(mutex);

wait(ws);     // if it is active writer then it will pass through else it will not

  <CRITICAL SECTION> // WRITING.....

wait(mutex);
wait(aw); // writing is done so decrement

if(aw==0&&wr>0){ // All writers are done and readers are waiting
   while (wr>0)  // Allowing all waiting readers to enter
        {
           signal(rs);  // wake a writer;
           signal(ar);  // increment to active readers
           wait(wr); // decrement to waiting readers
       }
}
signal(mutex);
}
  
Here,
wait(a){
while(a<=0)
    ;// no-op
  a--;
}
           
signal(a){
a++;
}
