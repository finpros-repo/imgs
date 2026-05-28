# 1. TỔNG QUÁT
Test alpha là công đoạn quan trọng đảm bảo chất lượng alpha trước khi gửi lên server để **papertrade**, test alpha gồm 2 bước:
- Backtest: Kiểm tra thông số alpha và vẽ PNL
- Test trước submit: Kiểm tra điều kiện futures leak và overfit của alpha trước khi submit lên server.

# 2. BACKTEST {#backtest-alpha}
Thực hiện backtest bao gồm 2 bước sau:
## 2.1. Khởi tạo đối tượng backtest
Sau khi có đối tượng **alpha** và đã gọi phương thức `generate()` để tính ra vị thế, bạn cần khởi tạo đối tượng backtest:
```python
bt = alpha.backtest()
```
## 2.2. Tính toán thông số và vẽ PNL
Gọi phương thức `Plot_PNL()` của đối tượng **bt** vừa khởi tạo ở trên để thực hiện tính toán thông số alpha và vẽ PNL cho alpha
```python
bt.Plot_PNL()
```
Output:
```bash
                   Margin: 37.28
         Margin after fee: 37.28
                      MDD: 14217.91 (89.05%); Time: 2021-02-22 → 2021-02-26
   Total trading quantity: 1315
         Sharpe after fee: 3.35
         Profit per trade: 151.62
             Total Profit: 199383.62
         Profit after fee: 199382.7
 Trading quantity per day: 1.52
 Profit per day after fee: 230.77
          Profit per year: 84311.38
                  HitRate: 45.32%
            Daily HitRate: 62.04%
           Weekly HitRate: 66.20%
          Monthly HitRate: 69.81%
                     Long: 657
                    Short: 658
             Hitrate long: 45.81%
            Hitrate short: 44.83%
Longest consecutive losing days: 7
        Overnight holding: 100.0%
```

<p align="center">
  <img src="https://github.com/finpros-repo/imgs/blob/main/pnl_backtest.png?raw=true"/>
  <br>
  PNL Backtest
</p>

# 3. TEST ALPHA TRƯỚC SUBMIT {#test-alpha}
Nếu thông số alpha tốt ở bước trên và bạn muốn nộp alpha lên server, bạn sẽ cần pass qua một bài test nữa để kiểm tra hai điều kiện *futures leak* và *overfit* của alpha.

Gọi phương thức `test_alpha()` của đối tượng **alpha**:
```python
alpha.test_alpha()
```
Output:
```bash
Checking futures leak...
     Pass check futures leak
Checking overfit...
     Pass check overfit
```
Chỉ cần 1 trong 2 bài test không pass, bạn cần kiểm tra, sửa lại logic alpha và thực hiện [backtest](#backtest-alpha), [test](#test-alpha) lại.

Nếu kết quả 2 bài test đều pass, alpha của bạn đủ điều kiện submit. 
