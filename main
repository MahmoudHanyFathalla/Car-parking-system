library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity parking_system is
	 GENERIC(
		  clk_freq   :  INTEGER := (40));
    port (
        ticket_in : in integer;
        enter_time_in : in integer;
        exit_time_in : in integer;
        clk : in std_logic;
        payment_out : out integer;
        full_out : out std_logic;
        id_valid : in std_logic;
        car_entering : in std_logic;
        car_exiting : in std_logic;
        gate_open : out std_logic;
		  ----------------------------------------------
        rst: in std_logic;
        occupy: inout std_logic_vector(49 downto 0);  -- modified range of occupy vector
        led: out std_logic_vector(49 downto 0);  -- modified range of led vector
        free_count: inout natural;
        occupied_count: inout natural;
        temperature: in natural;  -- new input port for temperature sensor
        fire: out std_logic;  -- new output port for fire indicator
        coffee_button: in std_logic;  -- new input port for coffee button
        hot_chocolate_button: in std_logic;  -- new input port for hot chocolate button
        ice_cream_button: in std_logic;  -- new input port for ice cream button
        water_button: in std_logic;  -- new input port for water button
        coffee_ready: inout std_logic;  -- new output port for coffee ready indicator
        hot_chocolate_ready: inout std_logic;  -- new output port for hot chocolate ready indicator
        ice_cream_ready: inout std_logic;  -- new output port for ice cream ready indicator
        water_ready: inout std_logic;  -- new output port for water ready indicator
        money_received: in std_logic;  -- new input port for money received indicator
        change_amount: out natural;  -- new output port for change amount
		  ----------------------------------------------
        seg : out std_logic_vector(6 downto 0);
        seg2 : out std_logic_vector(6 downto 0);
		  ----------------------------------------------            
		  sensor_pw  :  IN   STD_LOGIC_VECTOR(49 DOWNTO 0);    
		  carexist   :  OUT  STD_LOGIC_VECTOR(49 DOWNTO 0);
		  placement  :  IN   natural Range 0 to 49;
		  check      :  out  integer;
		  check_count      :  out  natural := 0;
		  distance   :  INOUT  STD_LOGIC_VECTOR(7 DOWNTO 0)
    );
end entity parking_system;

architecture behavior of parking_system is
    type guest is record
        ticket : integer;
        enter_time : integer;
        exit_time : integer;	  
    end record;
    
    type guest_array is array (natural range <>) of guest;
    
    constant max_guests : natural := 100; -- maximum number of guests
    signal guests : guest_array(0 to max_guests-1); -- array of guests
    signal guest_count : natural := 0; -- number of guests currently in the garage
	 signal guest_charge : real;
	 -------------------------------------------------
    -- constants for drink prices
    constant coffee_price: natural := 50;
    constant hot_chocolate_price: natural := 75;
    constant ice_cream_price: natural := 100;
    constant water_price: natural := 25;

    -- internal signal for current drink selection
    signal current_drink: std_logic_vector(2 downto 0);
	 -------------------------------------------------
	 SIGNAL sensor_pw_prev  :  STD_LOGIC_VECTOR(49 DOWNTO 0);

    
begin
    
    -- process to handle a new guest entering the garage without id
    process (ticket_in, enter_time_in, id_valid, car_entering, clk)
    begin
			if (clk'event and clk = '1') then
				if ticket_in /= 0 and guest_count < max_guests and id_valid='0' and car_entering ='1' and car_exiting ='0' then
					guests(guest_count).ticket <= ticket_in;
					guests(guest_count).enter_time <= enter_time_in;
					guests(guest_count).exit_time <= 0;
					payment_out <= 0;
					guest_count <= guest_count + 1;
					gate_open <='1';
					check_count <= guest_count;
					
				elsif  guest_count < max_guests and id_valid='1' and car_entering ='1' and car_exiting ='0' then
					guest_count <= guest_count + 1;
					gate_open <='1';
					check_count <= guest_count;
					
				elsif id_valid='1' and car_entering ='0' and car_exiting ='1' then
					guest_count <= guest_count - 1;
					gate_open <='1';
					check_count <= guest_count;
					
				elsif (id_valid='1' or id_valid='0') and car_entering ='0' and car_exiting ='0' then
						gate_open <='0';

					
					else
        for i in 0 to max_guests -1 loop
				if ticket_in /= 0 and id_valid='0' and car_exiting ='1' and car_entering ='0' and guests(i).ticket = ticket_in then
					guests(i).exit_time <= exit_time_in;
					payment_out <= (exit_time_in - (guests(i).enter_time)) * 2;
					guest_count <= guest_count - 1;
					gate_open <='1';
					check_count <= guest_count;
					end if;
							end loop;
				end if;
			end if;

    end process;
	 
	 PROCESS(clk,rst)
    VARIABLE cm_timecount  :  INTEGER := 0;  
    VARIABLE cm_counter      :  INTEGER := 0;  
	 type dist_array is array (0 to 49) of std_logic_vector(7 downto 0);
	 variable distances: dist_array;
	 BEGIN
     IF(rst = '1') THEN    
		 led <= (others => '1');
       free_count <= 50;
       occupied_count <= 0;
       fire <= '0';  -- reset fire indicator to 0
       coffee_ready <= '0';  -- reset coffee ready indicator to 0
       hot_chocolate_ready <= '0';  -- reset hot chocolate ready indicator to 0
       ice_cream_ready <= '0';  -- reset ice cream ready indicator to 0
       water_ready <= '0';  -- reset water ready indicator to 0
       change_amount <= 0;  -- reset change amount to 0	  
       cm_timecount := 0;                             
       cm_counter := 0;      
       distance <= (OTHERS => '0'); 
	    carexist <= (OTHERS => '0'); 	
		 occupy <= (OTHERS => '0'); 
     ELSIF(clk'EVENT AND clk = '1') THEN   
			 sensor_pw_prev(placement) <= sensor_pw(placement);                       
			 IF(sensor_pw(placement) = '1' AND (placement + 1) <= guest_count) THEN                           
				IF(cm_timecount < (29/1000000)*clk_freq) THEN          
				  cm_timecount := cm_timecount + 1;          
				ELSE                                              
				  cm_timecount := 0;                             
				  cm_counter := cm_counter + 1;                  
				END IF;
			 END IF;
			 IF(sensor_pw_prev(placement) = '1' AND sensor_pw(placement) = '0' AND (placement + 1) <= guest_count) THEN
				  check <= cm_counter;
				  distance <= std_logic_vector(to_unsigned(cm_counter,distance'LENGTH));
				  distances(placement) := distance;
				  if(cm_counter < 20000) then
					 carexist(placement) <= '1';
					 occupy(placement) <= '1';
					 led(placement) <= '0'; 
					 occupied_count <= occupied_count + 1;
					 free_count <= free_count - 1;
				  end if;
				  cm_timecount := 0;
				  cm_counter := 0;
			 END IF;
			 IF(sensor_pw_prev(placement) = '0' AND sensor_pw(placement) = '1' AND distances(placement) > "00000000" AND (placement + 1) <= guest_count) then
				 carexist(placement) <= '0';
				 occupy(placement) <= '0';
				 led(placement) <= '1';
				 free_count <= free_count + 1;
				 occupied_count <= occupied_count - 1;
		    END IF;
		 if (temperature > 300) then  -- threshold temperature for fire detection
                fire <= '1';
            else
                fire <= '0';
            end if;
            -- update current drink selection based on button inputs
            -- update current drink selection based on button inputs
				if (coffee_button = '1') then
					 current_drink <= "000";
					 coffee_ready <= '1';
					 hot_chocolate_ready <= '0';
					 ice_cream_ready <= '0';
					 water_ready <= '0';
				elsif (hot_chocolate_button = '1') then
					 current_drink <= "001";
					 coffee_ready <= '0';
					 hot_chocolate_ready <= '1';
					 ice_cream_ready <= '0';
					 water_ready <= '0';
				elsif (ice_cream_button = '1') then
					 current_drink <= "010";
					 coffee_ready <= '0';
					 hot_chocolate_ready <= '0';
					 ice_cream_ready <= '1';
					 water_ready <= '0';
				elsif (water_button = '1') then
					 current_drink <= "011";
					 coffee_ready <= '0';
					 hot_chocolate_ready <= '0';
					 ice_cream_ready <= '0';
					 water_ready <= '1';
				else
					 -- no button was pressed, so keep the current drink selection and ready indicators unchanged
					 coffee_ready <= coffee_ready;
					 hot_chocolate_ready <= hot_chocolate_ready;
					 ice_cream_ready <= ice_cream_ready;
					 water_ready <= water_ready;
				end if;

				
            -- calculate and output change amount based on money received
            case current_drink is
                when "000" =>
                    if (money_received = '1') then
                        change_amount <= coffee_price;
                    end if;
                when "001" =>
                    if (money_received = '1') then
                        change_amount <= hot_chocolate_price;
                    end if;
                when "010" =>
                    if (money_received = '1') then
                        change_amount <= ice_cream_price;
                    end if;
                when "011" =>
                    if (money_received = '1') then
                        change_amount <= water_price;
                    end if;
                when others =>
                    change_amount <= 0;
            end case;
     END IF;
    END PROCESS;



   
	 
    
    -- output indicating whether the garage is full
    process (guest_count)
    begin
    full_out <= '0';
        if guest_count = max_guests then
            full_out <= '1';
        end if;
    end process;
	 ------------------------------------------------------------
	 process (free_count)
	 begin
			if (free_count = 0) then
				 seg <= "1111110";
				 seg2 <= "1111110";
				elsif (free_count = 1) then
				seg<="0110000";
				seg2 <= "1111110";
				elsif (free_count = 2) then
				seg<="1101101";
				seg2 <= "1111110";
				elsif (free_count = 3) then
				seg<="1111001";
				seg2 <= "1111110";
				elsif (free_count = 4) then
				seg<="0110011";
				seg2 <= "1111110";
				elsif (free_count = 5) then
				seg<="1011111";
				seg2 <= "1111110";
				elsif (free_count = 6) then
				seg<="1011111";
				seg2 <= "1111110";
				elsif (free_count = 7) then
				seg<="1110000";
				seg2 <= "1111110";
				elsif (free_count = 8) then
				seg<="1111111";
				seg2 <= "1111110";
				elsif (free_count = 9) then
				seg<="1111011";
				seg2 <= "1111110";
				elsif (free_count = 10) then
				 seg <= "1111110";
				 seg2<="0110000";
				elsif (free_count = 11) then
				seg<="0110000";
				seg2<="0110000";
				elsif (free_count = 12) then
				seg<="1101101";
				seg2<="0110000";
				elsif (free_count = 13) then
				seg<="1111001";
				seg2<="0110000";
				elsif (free_count = 14) then
				seg<="0110011";
				seg2<="0110000";
				elsif (free_count = 15) then
				seg<="1011111";
				seg2<="0110000";
				elsif (free_count = 16) then
				seg<="1011111";
				seg2<="0110000";
				elsif (free_count = 17) then
				seg<="1110000";
				seg2<="0110000";
				elsif (free_count = 18) then
				seg<="1111111";
				seg2<="0110000";
				elsif (free_count = 19) then
				seg<="1111011";
				seg2<="0110000";
				elsif (free_count = 20) then
				 seg <= "1111110";
				 seg2<="1101101";
				elsif (free_count = 21) then
				seg<="0110000";
				seg2<="1101101";
				elsif (free_count = 22) then
				seg<="1101101";
				seg2<="1101101";
				elsif (free_count = 23) then
				seg<="1111001";
				seg2<="1101101";
				elsif (free_count = 24) then
				seg<="0110011";
				seg2<="1101101";
				elsif (free_count = 25) then
				seg<="1011111";
				seg2<="1101101";
				elsif (free_count = 26) then
				seg<="1011111";
				seg2<="1101101";
				elsif (free_count = 27) then
				seg<="1110000";
				seg2<="1101101";
				elsif (free_count = 28) then
				seg<="1111111";
				seg2<="1101101";
				elsif (free_count = 29) then
				seg<="1111011";
				seg2<="1101101";
				elsif (free_count = 30) then
				 seg <= "1111110";
				 seg2<="1111001";
				elsif (free_count = 31) then
				seg<="0110000";
				seg2<="1111001";
				elsif (free_count = 32) then
				seg<="1101101";
				seg2<="1111001";
				elsif (free_count = 33) then
				seg<="1111001";
				seg2<="1111001";
				elsif (free_count = 34) then
				seg<="0110011";
				seg2<="1111001";
				elsif (free_count = 35) then
				seg<="1011111";
				seg2<="1111001";
				elsif (free_count = 36) then
				seg<="1011111";
				seg2<="1111001";
				elsif (free_count = 37) then
				seg<="1110000";
				seg2<="1111001";
				elsif (free_count = 38) then
				seg<="1111111";
				seg2<="1111001";
				elsif (free_count = 39) then
				seg<="1111011";
				seg2<="1111001";
				elsif (free_count = 40) then
				 seg <= "1111110";
				 seg2<="0110011";
				elsif (free_count = 41) then
				seg<="0110000";
				seg2<="0110011";
				elsif (free_count = 42) then
				seg<="1101101";
				seg2<="0110011";
				elsif (free_count = 43) then
				seg<="1111001";
				seg2<="0110011";
				elsif (free_count = 44) then
				seg<="0110011";
				seg2<="0110011";
				elsif (free_count = 45) then
				seg<="1011111";
				seg2<="0110011";
				elsif (free_count = 46) then
				seg<="1011111";
				seg2<="0110011";
				elsif (free_count = 47) then
				seg<="1110000";
				seg2<="0110011";
				elsif (free_count = 48) then
				seg<="1111111";
				seg2<="0110011";
				elsif (free_count = 49) then
				seg<="1111011";
				seg2<="0110011";
				elsif (free_count = 50) then
				 seg <= "1111110";
				 seg2<="1011111";
				end if;
end process;
    
end architecture behavior;
