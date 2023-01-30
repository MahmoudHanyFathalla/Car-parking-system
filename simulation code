library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity parking_system_tb is
end entity;

architecture behavior of parking_system_tb is
    -- signals for testing
    signal ticket_in: integer;
    signal enter_time_in: integer;
    signal exit_time_in: integer;
    signal clk: std_logic;
    signal payment_out: integer;
    signal full_out: std_logic;
    signal id_valid: std_logic;
    signal car_entering: std_logic;
    signal car_exiting: std_logic;
    signal gate_open: std_logic;
    signal rst: std_logic;
    signal occupy: std_logic_vector(49 downto 0);
    signal led: std_logic_vector(49 downto 0);
    signal free_count: natural;
    signal occupied_count: natural;
    signal temperature: natural;
    signal fire: std_logic;
    signal coffee_button: std_logic;
    signal hot_chocolate_button: std_logic;
    signal ice_cream_button: std_logic;
    signal water_button: std_logic;
    signal coffee_ready: std_logic;
    signal hot_chocolate_ready: std_logic;
    signal ice_cream_ready: std_logic;
    signal water_ready: std_logic;
    signal money_received: std_logic;
    signal change_amount: natural;
    signal seg : std_logic_vector(6 downto 0);
    signal seg2 : std_logic_vector(6 downto 0);
    SIGNAL sensor_pw  :  STD_LOGIC_VECTOR(49 DOWNTO 0) := (OTHERS => '0');
    SIGNAL carexist   :  STD_LOGIC_VECTOR(49 DOWNTO 0);
    SIGNAL distance   :  STD_LOGIC_VECTOR(7 DOWNTO 0);
    SIGNAL placement  :  natural Range 0 to 49;
    SIGNAL check      :  integer;
	 SIGNAL check_count      :  natural := 0;
    
    -- component to be tested
    component parking_system is
	     GENERIC(
            clk_freq   :  INTEGER := 40);
        port (
            ticket_in : in integer;
            enter_time_in : in integer;
            exit_time_in : in integer;
            clk : in std_logic;
            payment_out : out integer;
            full_out : out std_logic;
            id_valid : in std_logic;
            car_entering : inout std_logic;
            car_exiting : inout std_logic;
            gate_open : out std_logic;
            rst: in std_logic;
            occupy: in std_logic_vector(49 downto 0);
            led: out std_logic_vector(49 downto 0);
            free_count: inout natural;
            occupied_count: inout natural;
            temperature: in natural;
            fire: out std_logic;
            coffee_button: in std_logic;
            hot_chocolate_button: in std_logic;
            ice_cream_button: in std_logic;
            water_button: in std_logic;
            coffee_ready: inout std_logic;
            hot_chocolate_ready: inout std_logic;
            ice_cream_ready: inout std_logic;
            water_ready: inout std_logic;
            money_received: in std_logic;
            change_amount: out natural;
				seg : out std_logic_vector(6 downto 0);
				seg2 : out std_logic_vector(6 downto 0);
			 	sensor_pw  :  IN   STD_LOGIC_VECTOR(49 DOWNTO 0);    
		      carexist   :  OUT  STD_LOGIC_VECTOR(49 DOWNTO 0);
		      placement  :  IN   natural Range 0 to 49;
		      check      :  out  integer;
		      distance   :  INOUT  STD_LOGIC_VECTOR(7 DOWNTO 0);
				check_count      :  out  natural:= 0
        );
    end component;
	 
	 CONSTANT clk_period : time := 10 ns;
	 
	 begin
    -- instantiate the design under test
    dut: entity work.parking_system
        port map (
            clk => clk,
            rst => rst,
				ticket_in => ticket_in,
				enter_time_in => enter_time_in,
				exit_time_in => exit_time_in,
				payment_out => payment_out,
				full_out => full_out,
				id_valid => id_valid,
				car_entering => car_entering,
				car_exiting => car_exiting,
				gate_open => gate_open,
            occupy => occupy,
            led => led,
            free_count => free_count,
            occupied_count => occupied_count,
            temperature => temperature,
            fire => fire,
            coffee_button => coffee_button,
            hot_chocolate_button => hot_chocolate_button,
            ice_cream_button => ice_cream_button,
            water_button => water_button,
            coffee_ready => coffee_ready,
            hot_chocolate_ready => hot_chocolate_ready,
            ice_cream_ready => ice_cream_ready,
            water_ready => water_ready,
            money_received => money_received,
            change_amount => change_amount,
				seg => seg,
				seg2 => seg2,
				sensor_pw  => sensor_pw,
			   carexist   => carexist,
			   distance   => distance,
			   placement  => placement,
			   check      => check,
				check_count => check_count
        );

    -- clock generation process
    clk_process: process
    begin
        clk <= '0';
        wait for clk_period/2;
        clk <= '1';
        wait for clk_period/2;
    end process;

    -- test scenario
    test_scenario: process
    begin
        -- initialize test input signals
        rst <= '1';
        wait for 10 ns;
        rst <= '0';
        wait for 10 ns;

        gate_open <= '0';
        wait for 10 ns;
        
        -- set temperature to a high value to trigger fire indicator
        temperature <= 400;
        wait for 10 ns;
        -- set temperature back to a normal value
        temperature <= 200;
        wait for 10 ns;

        -- press coffee button and pay for drink
        coffee_button <= '1';
        money_received <= '1';
        wait for 10 ns;
        coffee_button <= '0';
        money_received <= '0';
        wait for 10 ns;

        -- press hot chocolate button and pay for drink
        hot_chocolate_button <= '1';
        money_received <= '1';
        wait for 10 ns;
        hot_chocolate_button <= '0';
        money_received <= '0';
        wait for 10 ns;

        -- press ice cream button and pay for drink
        ice_cream_button <= '1';
        money_received <= '1';
        wait for 10 ns;
        ice_cream_button <= '0';
        money_received <= '0';
        wait for 10 ns;

        -- press water button and pay for drink
        water_button <= '1';
        money_received <= '1';
        wait for 10 ns;
        water_button <= '0';
        money_received <= '0';
        wait for 10 ns;
		  
		  car_entering <= '1';
		  id_valid <= '1';
		  car_exiting <= '0';
		  wait for 10 ns;
		  car_entering <= '0';
		  wait for 10 ns;
		  car_entering <= '1';
		  id_valid <= '0';
		  ticket_in <= 5;
		  enter_time_in <= 3;
		  car_exiting <= '0';
		  wait for 10 ns;
		  car_entering <= '0';
		  wait for 10 ns;
		  
		
		  
		 placement <= 0;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 0;
		 sensor_pw(placement) <= '0';
		wait for clk_period*2;
		 placement <= 0;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 0;
		 sensor_pw(placement) <= '0';
		wait for clk_period*2;
		 placement <= 1;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 1;
		 sensor_pw(placement) <= '0';
		wait for clk_period*2;
		 placement <= 1;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 1;
		 sensor_pw(placement) <= '0';
		wait for clk_period*2;
		 placement <= 0;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 2;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 2;
		 sensor_pw(placement) <= '0';
		wait for clk_period*2;
		 placement <= 2;
		 sensor_pw(placement) <= '1';
		wait for clk_period*2;
		 placement <= 2;
		 sensor_pw(placement) <= '0';
		wait for clk_period*2;
		
		

		 
		  
		  
        wait;
    end process;
end architecture;
