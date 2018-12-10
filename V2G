//VARIABLE INITILIZATION 

b_init = roundToDecimal(SOC,2);
b_des = roundToDecimal(Battery_capacity,2);
b_min = roundToDecimal(0.1*Battery_capacity,2);

traceln("pre check bds: " + b_des);



//SESSION LENGTH 

Start_date = date();
End_date = addToDate(date(), HOUR, Duration_charging_session );
traceln("soc: " + SOC);
traceln("battery capacity: " + Battery_capacity);
traceln("start time: " + Start_date);
traceln("End date: " + End_date);
traceln("Duration charging session: " + Duration_charging_session);


//if (Duration_charging_session > 23){
    
    //length_price_array = getHourOfDay(End_date) - getHourOfDay() + 1 + (int) Duration_charging_session;

//CREATING 15-MINUTES TIME ARRAY

for(int i=0; i<59; i++){

    if(getMinute() == 0){
    
      start_minute = 0;
    }
    else if(getMinute() < 16 && getMinute() > 0){
    
      start_minute = 0;
    }
    else if ( getMinute() < 31 && getMinute() > 15){
    
      start_minute = 1;
    }
    else if (getMinute() <46 && getMinute() > 30){ 
    
      start_minute = 2;
    } 
    else if (getMinute() < 60 && getMinute() > 45){
    
      start_minute = 3;
    } 
} 

for(int i=0; i<59; i++){

    if(getMinute(End_date) == 0){
    
      end_minute = 0;
    }
    else if(getMinute(End_date) < 16 && getMinute(End_date) > 0){
    
      end_minute = 1;
    }
    else if ( getMinute(End_date) < 31 && getMinute(End_date) > 15){
    
      end_minute = 2;
    }
    else if (getMinute(End_date) < 46 && getMinute(End_date) > 30){ 
    
      end_minute = 3;
    } 
    else if (getMinute(End_date) < 60 && getMinute(End_date) > 45){
    
      end_minute = 4;
    } 
} 

if (Duration_charging_session > 23){
    
    diff_end_start = getHourOfDay(End_date) - getHourOfDay() + 1 + (int) Duration_charging_session;
   }
else {

      diff_end_start = getHourOfDay(End_date) - getHourOfDay() + 1; 
      
}
if (diff_end_start == 1){

   if (Duration_charging_session == 0.0){
    
       new_length_time_array = 1;
	}
	else 
	{  
	   
	   diff_end_start = 25;
	}  

}

if (diff_end_start <= 0)
{ 
    diff_end_start = diff_end_start + 24;
}


new_length_time_array = (diff_end_start)*4 - (start_minute + (4 - end_minute));


start_minute1 = (4 - start_minute);
end_minute1 = (4 - end_minute);
traceln("start_minute: " + start_minute);
traceln("start_minute1: " + start_minute1);
traceln("endt_minute: " + end_minute);
traceln("end_minute1: " + end_minute1);

/*length_time_array = getHourOfDay(End_date) - getHourOfDay() + 1;
if(length_time_array <= 0) //When charging over night. It means ending hour is smaller then starting hour.
{
	length_time_array = length_time_array + 24;
}
*/
//TIME-ARRAY & PRICE-ARRAY INITILIZATION
time_array = new double[new_length_time_array];

//price_array = new double[length_time_array+1];

price_array_1 = new double[new_length_time_array+1];


for (int i=0; i<new_length_time_array; i++){ 
     
    time_array[i] = i+1;

}  
/*/GET PRICES FROM SPOTMARKET & ADD IT TO ARRAY

for (int i=0; i<length_time_array+1; i++){
     
    price_array[i] = (main.spotmarket_prices.get(i)/100);

}
*/
//NEW PRICE ARRAY INITIALIZATION
if (new_length_time_array > 1){
	
	traceln("check");
	for (int i=0; i<start_minute1; i++){

    	price_array_1[i] = (main.spotmarket_prices.get(0)/100);
    
   
	}

	for (int i=0; i<diff_end_start - 2; i++){

    	for (int j=start_minute1; j < (start_minute1 + 4); j++){
    
        	price_array_1[j + (i*4)] = (main.spotmarket_prices.get(i+1)/100);
        
        
    	}
	}

	for (int i=(new_length_time_array-end_minute); i<new_length_time_array; i++){

    	price_array_1[i] = (main.spotmarket_prices.get(diff_end_start-1)/100); 
	    
	} 

}

else

{   
    traceln("check23");
    for (int i=0; i<new_length_time_array+1; i++){
     
    price_array_1[i] = (main.spotmarket_prices.get(i)/100);

	}

}

//NODES & TIME-STEPS COLLECTIONS INITILIZATION 

if (Nodes.isEmpty())
{
   Nodes.add(b_init);
}

if (Time_steps.isEmpty()){
    
    Time_steps.add(0.0);
    traceln("timesteps: " + Time_steps);
}

//VARIABLE INITIALIZATION 2

/*/CHECK ENOUGH POWER AND TIME OR CHANGE B_DES & B_MIN
if ((Requested_charging_power * (length_time_array) + b_init) < b_des){
    
   b_des = (Requested_charging_power * (length_time_array)) + b_init;
   traceln("check bds");
   b_des = roundToDecimal(b_des,2);
}

if (b_des - (Requested_charging_power * (length_time_array)) > b_min){

    b_min = b_des - (Requested_charging_power * (length_time_array));
    b_min = roundToDecimal(b_min,2); 
} 
*/
//CHECK ENOUGH POWER AND TIME OR CHANGE B_DES & B_MIN FOR 15-MINUTES GRID
if ((Requested_charging_power/4 * (new_length_time_array) + b_init) < b_des){
    
   b_des = (Requested_charging_power/4 * (new_length_time_array)) + b_init;
   traceln("check bds");
   b_des = roundToDecimal(b_des,2);
}

if (b_des - (Requested_charging_power/4 * (new_length_time_array)) > b_min){

    b_min = b_des - (Requested_charging_power/4 * (new_length_time_array));
    b_min = roundToDecimal(b_min,2); 
} 

//ADD PRICES TO NEW SPOTMARKET COLLECTION
//spotmarket_prices1.add(price_array[0]);

//ADD PRICES TO NEW SPOTMARKET COLLECTION FOR 15-MINUTES GRID 
//for (int i=0; i<start_minute1; i++){

spotmarket_prices1.add(price_array_1[0]);  
//}

//DISPLAY
traceln("length_time_array: " + new_length_time_array);
traceln("time_array: " + Arrays.toString(time_array));
traceln("price_array: " + Arrays.toString(price_array_1));
traceln("Time_steps: "+ Time_steps);
traceln("b_des: "+ b_des);
traceln("Requested charging power: "+ Requested_charging_power);
traceln("minimum battery: "+ b_min);

for (int i=0; i<time_array.length + 1; i++){

//CHECK B_DES STEP VALUE DIFFERENCE 

if (b_init + Requested_charging_power/4 <= b_des)
{
   if (b_init + (Requested_charging_power/4 * i) <= b_des)
   {
      max_bat_pos = b_init + (Requested_charging_power/4 * i);
      if (b_des - max_bat_pos < Requested_charging_power/4)
      {
         b_des =  max_bat_pos;
         roundToDecimal(b_des,2);
         //traceln("B-max value: " + b_des);
         
      }
   }
}

}

for (int i=0; i<time_array.length + 1; i++){

//CHECK B_MIN STEP VALUE DIFF. & ENOUGH POWER & TIME

if (b_init - Requested_charging_power/4 >= b_min)
{
    if (b_init - (Requested_charging_power/4 * i) >= b_min)
    {   
       min_bat_pos = b_init - (Requested_charging_power/4 * i);
       if (min_bat_pos + ((time_array.length - i)*Requested_charging_power/4) < b_des)
       {   
           b_min = b_init - (Requested_charging_power/4 * (i-1));
           //traceln("B-min1 value: " + b_min); 
           b_min = roundToDecimal(b_min,2);
       }
       else if (min_bat_pos - b_min <= Requested_charging_power/4)
       {   
           b_min = min_bat_pos; 
           b_min = roundToDecimal(b_min,2);
           //traceln("B-min2 value: "+ b_min);
          
       }
     }
}
}

for (int i=0; i<time_array.length + 1; i++){

//MAX_BAT & MIN_BAT POSSIBILITY EACH TIME-STEP

if (b_init + Requested_charging_power/4 <= b_des)
{
   if (b_init + (Requested_charging_power/4 * i) <= b_des)
   {
      max_bat_pos = b_init + (Requested_charging_power/4 * i);
      traceln("check1");
      
   }
   else 
   { 
      max_bat_pos = b_des;
      traceln("check2");
   }
   
}

else 
{   
    
    max_bat_pos = b_init; 
    
    b_des = max_bat_pos; 
    
} 

//CHECKING POSSIBLE MINIMUM BATTERY LEVEL AT EVERY TIME STEP   

if (b_init - Requested_charging_power/4 >= b_min)
{
    if (b_init - (Requested_charging_power/4 * i) >= b_min)
    {   
       min_bat_pos = b_init - (Requested_charging_power/4 * i);
       min_bat_pos = roundToDecimal(min_bat_pos,2);
       
       traceln("check3");
    }

    else 
    {   
       
       if ((((time_array.length) - i) * Requested_charging_power/4) + b_min <= b_des)
       { 
          min_bat_pos = b_des - (((time_array.length) - i) * Requested_charging_power/4);
          traceln("check4");
          //traceln("Check10");
       } 
       else 
       {   
          min_bat_pos = b_min;
          traceln("check5");
          //traceln("check11");
       }
     }
}

else 
{   
     if ((time_array.length - i) * Requested_charging_power/4 + b_init > b_des)
     {
         min_bat_pos = b_init;
         traceln("check6");
         
     }
     else 
     {
         min_bat_pos = b_des - ((time_array.length - i) * Requested_charging_power/4);
         traceln("check7");
     }
}

//NUMBERS OF NODES AT PARTICULAR TIMESTEP

nodes_length = roundToInt(((max_bat_pos - min_bat_pos)/(Requested_charging_power/4)) + 1) ;

//DISPLAY
traceln("max battery: "+ max_bat_pos);
traceln("min battery: "+ min_bat_pos);
traceln("Nodes length: "+ nodes_length);

//CREAT NODE POINTS AND STORE IT

//CREATING NODE POINTS FROM BATTERY STATE LEVELS & STORE IT IN COLLECTION

for (double j = max_bat_pos; j >= min_bat_pos-1; j -= Requested_charging_power/4){
     
     check = j;
     naya = roundToDecimal((max_bat_pos - Requested_charging_power/4),2);
     //traceln("check: " + check);
     //traceln("max_bat_pos :" + naya);
     //traceln("min_bat_pos-1: " + (min_bat_pos - 1));
     Nodes.add(roundToDecimal(j,2));
     
}

//DISPLAY
//traceln("Nodes: " + Nodes);


//traceln("sum12: " + sum12);

//TIME-STEPS BASED ON NODES 

//CREATING TIME-STEPS COLLECTION 

for (int j = 0; j < (int) round(nodes_length); j++ ){
    
    time_array1 = time_array[i-1];
    Time_steps.add(time_array1);
    //traceln("time array check: " + time_array1);
}

//DISPLAY
//traceln("Time steps: " + Time_steps);

//SPOTMARKET PRICES BASED ON NODES

for (int j = 0; j < nodes_length; j++ )
{
    
    price_array1 = price_array_1[i];
    spotmarket_prices1.add(price_array1);
}

//DISPLAY
//traceln("spotmarket prices: " + spotmarket_prices1);

//TOTAL NODES IN THE GRAPH


Graph_matrix = new double[Nodes.size()][Nodes.size()];

traceln("Node size: "+ Nodes.size()); 
traceln("Time steps size: "+ Time_steps.size());

for(int k=0; k < Graph_matrix.length; k++){ 

   for(int j=0; j < Graph_matrix.length; k++){
   
   //GIVE WEIGHT TO EDGES

if (Time_steps.get(k) >= Time_steps.get(j))
{   
   Graph_matrix[k][j] = Double.NaN;
} 

else if (Time_steps.get(j) == Time_steps.get(k) + 1) 
{       
        if (Nodes.get(j) == Nodes.get(k) + 0.0)
        {
             Graph_matrix[k][j] = 0.0;
                
        }
        
        else if (Math.abs(Nodes.get(j) - (Nodes.get(k) + (Requested_charging_power/4))) < epsilon){
           
           if (spotmarket_prices1.get(k) < 0.00){
              
               Graph_matrix[k][j] = -1 * (spotmarket_prices1.get(k));
               //traceln("check01: ");
           }    
           
           else{ 
        
               Graph_matrix[k][j] = 1 * (spotmarket_prices1.get(k));
               //traceln("check11: ");
           }
        }
        
        else if (Math.abs(Nodes.get(j) - (Nodes.get(k) - (Requested_charging_power/4))) < epsilon){
        
             if (spotmarket_prices1.get(k) < 0.00){
             
                 Graph_matrix[k][j] = 1 * (spotmarket_prices1.get(k));
                 //traceln("check21: ");
                
             }
             
             else{
                 Graph_matrix[k][j] = -1 * (spotmarket_prices1.get(k));
                 //traceln("check22: ");
             }
           
        }
        
        else 
        {
           Graph_matrix[k][j] = Double.NaN;
           
        } 
}      

else
{
    Graph_matrix[k][j] = Double.NaN;
}
}
}

//BACKWARD INDUCTION ALGORITHM

//VARIABLE INTIALIZATION 

value_array = new double[Graph_matrix.length];

optimal_path = new double[Graph_matrix.length];

optimal_policy = new double[Graph_matrix.length-1];

trans_values = new double[Graph_matrix.length];

trans_policy = new double[Graph_matrix.length -1];

traceln("CHECK0");

//THE BELLMAN EQUATION 

value_array[Graph_matrix.length -1] = 0; 

//traceln("CHECK1");
//traceln("graph matrix length: " + Graph_matrix.length);

for (int s = Graph_matrix.length - 2; s > -1; s--)
{

    num_connections = 0; // number of connections from s
    
    //traceln("CHECK2");
    
    for (int n = s+1; n < Graph_matrix.length ; n++)
    {
    
        //traceln("s=" + s + ". n=" + n);
        
        if (Graph_matrix[s][n] > -10.0)
        {
           traceln("if check: " + "["+s+"]["+n+"]" + Graph_matrix[s][n]);
           
           num_connections = num_connections + 1 ;
       
           trans_values[num_connections-1] = Graph_matrix[s][n] + value_array[n] ;
        
           trans_policy[num_connections-1] = n ;
           
           traceln("trans_values: " + Arrays.toString(trans_values));
    
           traceln("trans_policy: " + Arrays.toString(trans_policy));
           
           
        }
       
    }
       //traceln("number of connections: " + num_connections);
       
      
      minValue = trans_values[0];
      index = 0;
       
      for (int i=0; i<num_connections; i++)
      {
           if (trans_values[i] < minValue)
           {
              minValue = trans_values[i];
              index = i;
              
         
           }
      }
    
      value_array[s] = minValue;
      minimizer = index;
 
      //traceln("value array: " + value_array[s]);
    
      //traceln("minimizer: " + minimizer);
    
      optimal_policy[s] = trans_policy[minimizer];
      traceln("Optimal policy array: " + Arrays.toString(optimal_policy));
}
//VARIABLE INITIALIZATION

optimal_path[0] = 0.0;
current_state = 0.0;
nodes_in_path = 0.0;
next_state = 0.0; 

while(true){
/ COMPUTE OPTIMAL PATH

next_state = optimal_policy[(int) current_state];

optimal_path[(int) nodes_in_path + 1] = next_state; 

nodes_in_path = nodes_in_path + 1;

if (next_state == Graph_matrix.length - 1) break;

current_state = next_state; 

traceln("nodes in path: " + nodes_in_path);
}
//OPTIMAL PATH ARRAY 

optimal_path1 = new double[(int) nodes_in_path + 1];
//traceln("nodes in the path: " + nodes_in_path);
for (int i=0; i < nodes_in_path + 1; i++){
     
     path = optimal_path[i];
     optimal_path1[i] = path;
}

traceln("Optimal path: " + Arrays.toString(optimal_path1));
traceln("optimal path length: " + optimal_path1.length);
//VARIABLE INITIALIZATION FOR V2G SCHEDULE

predicted_SOC = b_init;
kWh_scheduled = 0.0;
kWh_discharge = 0.0;
kWh_charge = 0.0;
Predicted_charging_power = 0.0;

//traceln("kwh charge 1: " + kWh_charge);
//traceln("kwh discharge 1: " + kWh_discharge); 


//IF V2G SCHEDULE IS EMPTY, CREATE NEW SCHEDULE


if(V2G_schedule.isEmpty())
{
	for( int i=0 ; i<96 ; i++)
	{
	    main.Index_V2G_schedule.add(0.0);
		V2G_schedule.add(0.0);
	}
}

traceln("fasdfsfa");

for(int i=0; i < time_array.length; i++){ 
//DETERMINE kWh TO CHARGE

kWh_charge_in_quarter = 0.0;
kWh_discharge_in_quarter = 0.0;

//kWh_charge_in_hour = 0.0;
//kWh_discharge_in_hour = 0.0;

p = (int) optimal_path1[i];
a = (int) optimal_path1[i+1];

//traceln("p: " + p);
//traceln("a: "+ a); 


index_quarter = ((int) time_array[i]-1 + getHourOfDay());
if (index_quarter > 95) {
   
   index_quarter = index_quarter - 96;
}
//traceln("get hour of day: "+getHourOfDay());
traceln("get hour of day: "+ (time_array[time_array.length -1] + getHourOfDay()));

traceln("index hour: "+ index_quarter);

if (Graph_matrix[p][a] > 0.00){ 
     
    if (index_quarter == getHourOfDay()){
    //if (index_hour == getHourOfDay()){
        
        kWh_in_quarter = Requested_charging_power/4 * (60 - getMinute()) / 60;
        kWh_charge_in_quarter = Requested_charging_power/4 * (60 - getMinute()) / 60;
        //kWh_in_hour = Requested_charging_power * (60 - getMinute()) / 60;
        //kWh_charge_in_hour = Requested_charging_power * (60 - getMinute()) / 60;
	    Predicted_charging_power = Requested_charging_power/4;
	    //traceln("check10");
	    traceln("kwh in hour: " + kWh_in_quarter); 
	    //traceln("kwh charge in hour: " + kWh_charge_in_hour);
    }
    else if(index_quarter == (time_array[time_array.length -1] + getHourOfDay()))
    //else if(index_hour == (getHourOfDay(End_date)))
    {   
        kWh_in_quarter = Requested_charging_power/4 * getMinute(End_date) / 60;
	    kWh_charge_in_quarter = Requested_charging_power/4 * getMinute(End_date) / 60;
	    //kWh_in_hour = Requested_charging_power * getMinute(End_date) / 60;
	    //kWh_charge_in_hour = Requested_charging_power * getMinute(End_date) / 60;
	    Predicted_charging_power = Requested_charging_power/4;
	    //traceln("check11");
	    traceln("kwh in hour: " + kWh_in_quarter);
	    //traceln("kwh charge in hour: " + kWh_charge_in_hour); 
    }
    else
    {   kWh_in_quarter = Requested_charging_power/4;
	    kWh_charge_in_quarter = Requested_charging_power/4;
	    //kWh_in_hour = Requested_charging_power;
	    //kWh_charge_in_hour = Requested_charging_power;
	    Predicted_charging_power = Requested_charging_power/4;
	    //traceln("check12");
	    traceln("kwh in hour: " + kWh_in_quarter); 
	    //traceln("kwh charge in hour: " + kWh_charge_in_hour);
    }
}
else if (Graph_matrix[p][a] < 0.00){
        
        if (index_quarter == getHourOfDay()){
        //if (index_hour == getHourOfDay()){
           
           kWh_in_quarter = -1 * (Requested_charging_power/4 * (60 - getMinute()) / 60);
           kWh_discharge_in_quarter = 1 * (Requested_charging_power/4 * (60 - getMinute()) / 60);
           //kWh_in_hour = -1 * (Requested_charging_power * (60 - getMinute()) / 60);
           //kWh_discharge_in_hour = 1 * (Requested_charging_power * (60 - getMinute()) / 60);
	       Predicted_charging_power = -1 * Requested_charging_power/4;
	       //traceln("check20");
	       traceln("kwh in hour: " + kWh_in_quarter); 
	       //traceln("kwh discharge in hour: " + kWh_discharge_in_hour);
        }
        else if(index_quarter == (time_array[time_array.length -1] + getHourOfDay()))
        //else if(index_hour == getHourOfDay(End_date))
        {  
           kWh_in_quarter = -1 * (Requested_charging_power/4 * getMinute(End_date) / 60);
	       kWh_discharge_in_quarter = 1 * (Requested_charging_power/4 * getMinute(End_date) / 60);
	       //kWh_in_hour = -1 * (Requested_charging_power * getMinute(End_date) / 60);
	       //kWh_discharge_in_hour = 1 * (Requested_charging_power * getMinute(End_date) / 60);
	       Predicted_charging_power = -1 * Requested_charging_power/4;
	       //traceln("check21");
	       traceln("kwh in hour: " + kWh_in_quarter); 
	       //traceln("kwh discharge in hour: " + kWh_discharge_in_hour);
        }
        else
        {  
           kWh_in_quarter = -1 * Requested_charging_power/4;
	       kWh_discharge_in_quarter = 1 * Requested_charging_power/4;
	       //kWh_in_hour = -1 * Requested_charging_power;
	       //kWh_discharge_in_hour = 1 * Requested_charging_power;
	       Predicted_charging_power = -1 * Requested_charging_power/4;
	       //traceln("check22");
	       traceln("kwh in hour: " + kWh_in_quarter); 
	       //traceln("kwh discharge in hour: " + kWh_discharge_in_hour);
        }
}
else
{   
    kWh_in_quarter = Requested_charging_power/4 * 0.00;
    //kWh_in_hour = Requested_charging_power * 0.00; 
    Predicted_charging_power = 0.0;
    traceln("kwh in hour: " + kWh_in_quarter); 
    //traceln("check30");
    kWh_discharge_in_quarter = Requested_charging_power/4 * 0.0;
    kWh_charge_in_quarter = Requested_charging_power/4 * 0.0; 
    //kWh_discharge_in_hour = Requested_charging_power/4 * 0.0;
    //kWh_charge_in_hour = Requested_charging_power/4 * 0.0; 
    //traceln("kwh discharge in hour: " + kWh_discharge_in_hour);
    //traceln("kwh charge in hour: " + kWh_charge_in_hour);
}
//UPDATE SOC & V2G SCHEDULE

predicted_SOC = predicted_SOC + kWh_in_quarter; 
kWh_scheduled = kWh_scheduled + kWh_in_quarter;
//V2G_schedule.set(index_hour, Predicted_charging_power);
V2G_schedule.set(index_quarter, Predicted_charging_power);
//main.Index_V2G_schedule.set(index_hour, kWh_in_hour); 
main.Index_V2G_schedule.set(index_quarter, kWh_in_quarter);

kWh_discharge = kWh_discharge + kWh_discharge_in_quarter;

kWh_charge = kWh_charge + kWh_charge_in_quarter; 

//traceln("kwh charge 2: " + kWh_charge);
//traceln("kwh discharge 2: " + kWh_discharge);
//traceln("kwh charge in hour 2: " + kWh_charge_in_hour);
//traceln("kwh discharge in hour 2: " + kWh_discharge_in_hour);


//traceln("V2G schedule: " + V2G_schedule); 

//DETERMINE KWH DESIRED AND SCHEDULED 

//TRIGGER CHARGE & PRICE CALCULATION

main.recalculate_spotmarket_prices();
send("Charge EV",this);

nb_schedules = nb_schedules + 1;

Scheduled_SOC = predicted_SOC;

//DETERMINE KWH WANTED AND SCHEDULED
kWh_wanted = min(Battery_capacity - b_init,Duration_charging_session * Requested_charging_power);
Total_wanted_kWh = Total_wanted_kWh + kWh_wanted;
Average_kWh_wanted_per_session = Total_wanted_kWh / nb_schedules;

Total_scheduled_kWh = Total_scheduled_kWh + kWh_scheduled;
main.kWh_scheduled = main.kWh_scheduled + kWh_scheduled;
Average_kWh_scheduled_per_session = Total_scheduled_kWh / nb_schedules;

V2G_schedule_graph.updateData(); 

Start_hour_text.setText("Start date charging session: " + Start_date);
Stop_hour_text.setText("End date charging session: " + End_date);
Duration_text.setText("Duration charging session: " + Duration_charging_session);
kWh_wanted_text.setText("kWh wanted in session: " + kWh_wanted);
kWh_scheduled_text.setText("kWh scheduled in session: " + kWh_scheduled);
Average_kWh_wanted_text.setText("Average kWh wanted per session: " + Average_kWh_wanted_per_session);
Average_kWh_scheduled_text1.setText("Average kWh scheduled per session: " + Average_kWh_scheduled_per_session);

traceln("Starting hour: " + getHourOfDay() + ".     Duration: " + Duration_charging_session);
traceln("Charge need: " + kWh_wanted + ".       kWh scheduled: " + kWh_scheduled + ".     Location: " + Current_CP_location);
traceln("Total_scheduled_kWh: " + Total_scheduled_kWh); 
traceln("Predicted SOC: " + predicted_SOC);
traceln("kwh discharge: " + kWh_discharge);
traceln("kwh charge: " + kWh_charge);

Nodes.clear();
Time_steps.clear(); 
spotmarket_prices1.clear();
spotmarket_prices.clear();
} 











