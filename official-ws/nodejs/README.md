# Node.JS Demo for Vbit

this is a demo for receiving realtime data and trade from the Vbit api.

## Usage  

   The following is runnable in [example.js](./example.js)

### To get started:
        npm install
        node example.js

### require lib.js:
        const Vbit = require('./lib/lib.js');
        const test_market_url = 'wss://cdn01.mcdzcloud.com/v1/market'; //this is the websocket url of simtrade;
        const test_trade_url  = 'wss://cdn01.mcdzcloud.com/v1/trade';

## Market:
### Init the client.
        var market = new Vbit();
        market.init({ws_url:test_market_url}, ()=>{});

### Subscribe to some data.
        market.request('Sub',["order20_"+sym],(ret)=>{});
        you can subscribe more data, like trade,tick,kline,orderl2

### Then you need to listen to this event.
        market.on("order20",function (ret){
                // Do something with the ret...
        });

### You can also take the initiative to get some data.
        market.request('GetAssetD',{}, (ret)=>{
                // Do something with the ret...
        });

## Trade:
*If you want to trade, you must to login to the official website to create an api key in the user center.  * 
        www.vbit.one

### Init the client.
        var trade = new Vbit();
        trade.init({ws_url:test_trade_url,SecretKey: secret_key}, ()=>{});

#### If the initialization is successful, You must be logged in to verify your identity in order to trade.
        var msg = {
                UserName: UserName,
                UserCred: api_key
        };
        trade.request('Login', msg, (ret)=>{
                // Be sure to save the returned UserId
        });

#### If you log in successfully, Vbit will actively push your private data to you, you only need to add some monitoring events.
        trade.on("onWallet",function (ret){
                // Do something with the ret...
        });
        You can listen to more data, like onOrder,onPosition,onTrade

#### You can take the initiative to send messages to trade and get data.
        trade.request('GetWallets',{AId: aid}, (ret)=>{
                // Do something with the ret... 
        });
        aid:UserId+"01";  // Future account
        aid:UserId+"02";  // Token trading account
   More interfaces can refer to [Vbit's api](../../WebSocket_API_v1.md)
