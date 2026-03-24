#stock_predict_basement
布林带（bollinger_bands）:就像你平时走路，正常会在马路中间走，偶尔偏左 / 偏右，但不会走到马路外。布林带就是给价格画一条 “中间路”（中轨），再画左右两条 “路边”（上轨、下轨）：
价格跑到 “路边外”（突破上轨 / 跌破下轨），大概率要回到 “中间路”；
“路边” 越窄，说明价格波动小，可能要大涨 / 大跌；越宽，说明波动大。
代码解析：import pandas as pd  # 先导入工具包（必须）

def bollinger_bands(close, window=20, num_std=2):
    # close：收盘价列（比如Excel里的一列数据）
    # window：看最近几天的数据（默认20天）
    # num_std：波动范围的“松紧度”（默认2倍）
    
    # 第一步：算“中间路”——最近window天的平均价格（比如20天均价）
    rolling_mean = close.rolling(window=window).mean()
    # 第二步：算“波动大小”——最近window天价格偏离均价的程度（标准差）
    rolling_std = close.rolling(window=window).std()
    # 第三步：算“右路边”——均价 + 2倍波动大小（上轨）
    upper_bb = rolling_mean + (rolling_std * num_std)
    # 第四步：算“左路边”——均价 - 2倍波动大小（下轨）
    lower_bb = rolling_mean - (rolling_std * num_std)
    
    return upper_bb, lower_bb  # 返回两条“路边”
RSI（compute_rsi）—— 算 “涨跌力度”：就像两个人拔河，RSI 就是算 “哪一方更用力”：
数值范围 0~100，50 是 “势均力敌”；
＞70：买方太用力（涨太猛，叫 “超买”，可能跌）；
＜30：卖方太用力（跌太狠，叫 “超卖”，可能涨）。
代码解析：def compute_rsi(close, window=14):
    # close：收盘价列
    # window：看最近14天的涨跌情况（默认14天，行业通用）
    
    # 第一步：算“每天涨跌多少”——今天价格 - 昨天价格
    delta = close.diff()  # 比如[10,11]的delta是1，[11,10.5]的delta是-0.5
    # 第二步：算“平均涨幅”——只把涨的天数加起来，算最近14天的平均值
    gain = (delta.where(delta > 0, 0)).rolling(window=window).mean()
    # 第三步：算“平均跌幅”——只把跌的天数加起来（取正数），算最近14天的平均值
    loss = (-delta.where(delta < 0, 0)).rolling(window=window).mean()
    # 第四步：算“涨跌力度比”——平均涨幅 ÷ 平均跌幅
    rs = gain / loss
    # 第五步：把力度比转成0~100的数（方便看）
    rsi = 100 - (100 / (1 + rs))
    
    return rsi
import akshare as ak
# 下载海航科技日K数据
stock_df = ak.stock_zh_a_hist(symbol=600751, period=daily, start_date=20251101, end_date=20260324, adjust=)
# 保存为CSV
stock_df.to_csv(海航科技日K数据.csv, index=False, encoding=utf-8-sig)
爬取股票专用工具，导出呈.csv文件
