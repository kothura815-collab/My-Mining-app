<!DOCTYPE html>
<html lang="my">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PTF Token Miner</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    body { background: #0f172a; color: #fff; font-family: sans-serif; text-align: center; margin: 0; padding: 20px; }
    .card { background: #1e293b; padding: 20px; border-radius: 12px; max-width: 400px; margin: auto; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
    h2 { font-size: 20px; margin-bottom: 5px; }
    #balance { font-size: 38px; color: #f59e0b; margin: 10px 0; }
    .hammer-btn { font-size: 70px; background: none; border: none; margin: 20px 0; cursor: pointer; transition: transform 0.1s; }
    .hammer-btn:active { transform: scale(0.9); }
    .btn { background: #22c55e; color: white; padding: 12px 24px; border: none; border-radius: 8px; width: 100%; font-size: 16px; cursor: pointer; font-weight: bold; margin-top: 10px; }
    .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); }
    .modal-content { background: #1e293b; padding: 20px; margin: 25% auto; width: 80%; max-width: 320px; border-radius: 10px; text-align: left; }
    input { width: 100%; padding: 10px; margin: 8px 0; border-radius: 5px; border: 1px solid #334155; background: #0f172a; color: white; box-sizing: border-box; }
    .extra-btns { display: flex; gap: 10px; margin-top: 15px; }
    .extra-btn { background: #334155; color: white; border: none; padding: 10px; border-radius: 8px; flex: 1; cursor: pointer; font-size: 13px; }
  </style>
</head>
<body>

  <div class="card">
    <h2>PTF TOKEN MINER</h2>
    <div id="balance">3.10</div>
    <p style="color: #4ade80; font-size: 14px;">System: Operational</p>
    
    <button class="hammer-btn" onclick="minePoint()">🔨</button>
    <p style="color: #94a3b8; margin-top: 0;">Tap to Mine PTF</p>

    <button class="btn" onclick="openModal()">💳 Withdraw PTF</button>

    <div class="extra-btns">
      <button class="extra-btn" onclick="alert('Daily Bonus Claimed!')">🎁 Daily Bonus</button>
      <button class="extra-btn" onclick="alert('Watching Ad...')">📺 Ad Reward</button>
    </div>
  </div>

  <!-- Withdraw Modal -->
  <div id="withdrawModal" class="modal">
    <div class="modal-content">
      <h3 style="margin-top:0; color:#f59e0b;">Payout Request</h3>
      <label style="font-size: 12px; color: #94a3b8;">আপনার নাম লিখুন</label>
      <input type="text" id="username" placeholder="Telegram Username">
      
      <label style="font-size: 12px; color: #94a3b8;">অ্যামাউন্ট (PTF)</label>
      <input type="number" id="amount" placeholder="Amount (PTF)">
      
      <label style="font-size: 12px; color: #94a3b8;">টন (TON) অ্যাড্রেস</label>
      <input type="text" id="wallet" placeholder="TON Wallet Address">
      
      <button class="btn" onclick="submitWithdrawal()" style="background:#22c55e; margin-top:15px;">SUBMIT REQUEST</button>
      <p style="text-align: center; cursor: pointer; color: #94a3b8; margin-top: 15px; font-size: 14px;" onclick="closeModal()">Cancel</p>
    </div>
  </div>

  <script>
    let points = 3.10;
    const tg = window.Telegram.WebApp;
    tg.expand(); // Telegram App ကို အပြည့်ချဲ့ရန်

    function minePoint() {
      points += 0.10;
      document.getElementById('balance').innerText = points.toFixed(2);
    }

    function openModal() { document.getElementById('withdrawModal').style.display = 'block'; }
    function closeModal() { document.getElementById('withdrawModal').style.display = 'none'; }

    async function submitWithdrawal() {
      const username = document.getElementById('username').value;
      const amount = document.getElementById('amount').value;
      const wallet = document.getElementById('wallet').value;

      if(!username || !amount || !wallet) {
        alert("ကျေးဇူးပြု၍ အချက်အလက်များကို ပြည့်စုံစွာဖြည့်ပါ။");
        return;
      }

      try {
        const res = await fetch('/api/withdraw', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, amount, wallet })
        });

        if(res.ok) {
          alert("Withdrawal Request Sent Successfully!");
          closeModal();
        } else {
          alert("Error sending request!");
        }
      } catch (e) {
        alert("Network error!");
      }
    }
  </script>
</body>
</html>
