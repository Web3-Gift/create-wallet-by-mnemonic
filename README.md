# create-wallet-by-mnemonic
这段 Python 代码也使用了 BIP39 和 BIP32 标准以从助记词生成 Ethereum 钱包，并且会将生成的钱包信息保存到 CSV 文件中。  请注意，要运行此代码，您需要先安装 bip-utils 和 pandas Python 库。您可以使用 pip install bip-utils pandas 命令进行安装。
from bip_utils import Bip44, Bip44Coins, Bip44Changes
import pandas as pd

# 填入助记词
mnemonic = "" 
# 设定生成数量
wallet_amount = 51

wallet_data = []  # 初始化数据，设置header

for i in range(wallet_amount):
    bip_obj_mst = Bip44.FromMnemonic(mnemonic, Bip44Coins.ETHEREUM)
    bip_obj_acc = bip_obj_mst.Purpose().Coin().Account(0)
    bip_obj_chain = bip_obj_acc.Change(Bip44Changes.CHAIN_EXT)
    bip_obj_addr = bip_obj_chain.AddressIndex(i)

    address = bip_obj_addr.PublicKey().ToAddress()
    privateKey = bip_obj_addr.PrivateKey().Raw().ToHex()

    wallet_data.append({"Wallet": f"wallet-{i}", "Address": address, "PrivateKey": privateKey})

# 将数据保存为CSV文件
df = pd.DataFrame(wallet_data)
df.to_csv('wallets.csv', index=False)
