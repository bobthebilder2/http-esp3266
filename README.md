# http-esp3266
srv = net.createServer(net.TCP)
srv:listen(80, function(conn)
    conn:on("receive", function(sck, payload)
        print(payload)
        sck:send("HTTP/1.0 200 OK\r\nContent-Type: text/html\r\n\r\n<h1> Hello, NodeMCU.</h1>")
    end)
    conn:on("sent", function(sck) sck:close() end)
end)

-- connect to WiFi access point (DO NOT save config to flash)
wifi.setmode(wifi.STATION)
station_cfg={}
station_cfg.ssid = "SSID"
station_cfg.pwd = "password"
station_cfg.save = false
wifi.sta.config(station_cfg)

-- register event callbacks for WiFi events
wifi.sta.eventMonReg(wifi.STA_CONNECTING, function(previous_state)
    if(previous_state==wifi.STA_GOTIP) then
        print("Station lost connection with access point. Attempting to reconnect...")
    else
        print("STATION_CONNECTING")
    end
end)

-- manipulate hardware like with Arduino
pin = 1
gpio.mode(pin, gpio.OUTPUT)
gpio.write(pin, gpio.HIGH)
print(gpio.read(pin))
