<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binance-Style Crypto Dashboard</title>
    <style>
        :root {
            --bg-color: #0B0E11;
            --card-bg: #161A1E;
            --text-primary: #EAECEF;
            --text-secondary: #848E9C;
            --positive: #0ECB81;
            --negative: #F6465D;
            --binance-yellow: #F0B90B;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .market-table {
            background: var(--card-bg);
            border-radius: 8px;
            overflow: hidden;
        }

        .table-header {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr 1fr 1fr;
            padding: 16px 24px;
            border-bottom: 1px solid #2B3139;
            font-weight: 500;
        }

        .table-row {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr 1fr 1fr;
            padding: 14px 24px;
            border-bottom: 1px solid #2B3139;
            transition: background 0.2s;
            cursor: pointer;
        }

        .table-row:hover {
            background: #1E2329;
        }

        .coin-info {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .coin-icon {
            width: 24px;
            height: 24px;
        }

        .price-up {
            color: var(--positive);
        }

        .price-down {
            color: var(--negative);
        }

        .search-bar {
            background: var(--card-bg);
            border: 1px solid #2B3139;
            padding: 12px 16px;
            border-radius: 6px;
            color: var(--text-primary);
            width: 300px;
        }

        .volume {
            color: var(--text-secondary);
        }

        /* Modal for graph */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
            z-index: 999;
        }

        .modal-content {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 8px;
            width: 80%;
            max-width: 900px;
            text-align: center;
        }

        .close-btn {
            color: #fff;
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 20px;
            cursor: pointer;
        }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Market Overview</h1>
            <input type="text" class="search-bar" placeholder="Search Coin...">
        </div>

        <div class="market-table">
            <div class="table-header">
                <span>Name</span>
                <span>Price</span>
                <span>24h Change</span>
                <span>24h Volume</span>
                <span>Market Cap</span>
                <span>Last 7 Days</span>
            </div>
            <div id="market-list"></div>
        </div>
    </div>

    <!-- Modal for Graph -->
    <div id="graphModal" class="modal">
        <div class="modal-content">
            <span class="close-btn">&times;</span>
            <h2 id="coinName">Coin Price History</h2>
            <canvas id="priceChart"></canvas>
        </div>
    </div>

    <script>
        const assets = [
            { id: 'bitcoin', symbol: 'btc', name: 'Bitcoin', type: 'coin' },
            { id: 'ethereum', symbol: 'eth', name: 'Ethereum', type: 'coin' },
            { id: 'binancecoin', symbol: 'bnb', name: 'BNB', type: 'coin' },
            { id: 'ripple', symbol: 'xrp', name: 'XRP', type: 'coin' },
            { id: 'cardano', symbol: 'ada', name: 'Cardano', type: 'coin' },
            { id: 'solana', symbol: 'sol', name: 'Solana', type: 'coin' },
            { id: 'polkadot', symbol: 'dot', name: 'Polkadot', type: 'coin' },
            { id: 'dogecoin', symbol: 'doge', name: 'Dogecoin', type: 'coin' },
            { id: 'avalanche-2', symbol: 'avax', name: 'Avalanche', type: 'coin' },
            { id: 'cosmos', symbol: 'atom', name: 'Cosmos', type: 'coin' },
            { id: 'litecoin', symbol: 'ltc', name: 'Litecoin', type: 'coin' },
            { id: 'chainlink', symbol: 'link', name: 'Chainlink', type: 'coin' },
            { id: 'monero', symbol: 'xmr', name: 'Monero', type: 'coin' },
            { id: 'stellar', symbol: 'xlm', name: 'Stellar', type: 'coin' },
            { id: 'bitcoin-cash', symbol: 'bch', name: 'Bitcoin Cash', type: 'coin' },
            { id: 'tether', symbol: 'usdt', name: 'Tether', type: 'token' },
            { id: 'usd-coin', symbol: 'usdc', name: 'USD Coin', type: 'token' },
            { id: 'shiba-inu', symbol: 'shib', name: 'Shiba Inu', type: 'token' },
            { id: 'uniswap', symbol: 'uni', name: 'Uniswap', type: 'token' },
            { id: 'aave', symbol: 'aave', name: 'Aave', type: 'token' },
            { id: 'compound-coin', symbol: 'comp', name: 'Compound', type: 'token' },
            { id: 'maker', symbol: 'mkr', name: 'Maker', type: 'token' },
            { id: 'the-graph', symbol: 'grt', name: 'The Graph', type: 'token' },
            { id: 'decentraland', symbol: 'mana', name: 'Decentraland', type: 'token' },
            { id: 'the-sandbox', symbol: 'sand', name: 'The Sandbox', type: 'token' },
            { id: 'axie-infinity', symbol: 'axs', name: 'Axie Infinity', type: 'token' },
            { id: 'curve-dao-token', symbol: 'crv', name: 'Curve DAO', type: 'token' },
            { id: 'yearn-finance', symbol: 'yfi', name: 'Yearn Finance', type: 'token' },
            { id: 'sushi', symbol: 'sushi', name: 'SushiSwap', type: 'token' }
        ];

        let marketDataCache = [];

        async function fetchMarketData() {
            try {
                const response = await fetch(
                    `https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=${assets.map(a => a.id).join(',')}&sparkline=true`
                );
                const data = await response.json();
                marketDataCache = data;
                updateMarketList(marketDataCache);
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        }

        function updateMarketList(marketData) {
            const marketList = document.getElementById('market-list');
            marketList.innerHTML = marketData.sort((a, b) => b.market_cap - a.market_cap).map(coin => {
                const asset = assets.find(a => a.id === coin.id);
                return `
                    <div class="table-row" data-coin-id="${coin.id}">
                        <div class="coin-info">
                            <img src="${coin.image}" class="coin-icon" alt="${coin.name}">
                            <div>
                                <div>${asset.name}</div>
                                <div style="color: var(--text-secondary);">${asset.symbol.toUpperCase()}/USD</div>
                            </div>
                        </div>
                        <div>$${coin.current_price.toLocaleString()}</div>
                        <div class="${coin.price_change_percentage_24h >= 0 ? 'price-up' : 'price-down'}">
                            ${coin.price_change_percentage_24h.toFixed(2)}%
                        </div>
                        <div class="volume">$${coin.total_volume.toLocaleString()}</div>
                        <div>$${coin.market_cap.toLocaleString()}</div>
                        <div>
                            <img src="${coin.sparkline_in_7d.image}" width="100" alt="sparkline">
                        </div>
                    </div>
                `;
            }).join('');
        }

        async function showGraph(coinId) {
            const coin = marketDataCache.find(c => c.id === coinId);
            document.getElementById('coinName').textContent = `${coin.name} Price History`;

            const response = await fetch(`https://api.coingecko.com/api/v3/coins/${coin.id}/market_chart?vs_currency=usd&days=7`);
            const data = await response.json();

            const labels = data.prices.map(price => new Date(price[0]).toLocaleDateString());
            const prices = data.prices.map(price => price[1]);

            const ctx = document.getElementById('priceChart').getContext('2d');
            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: `${coin.name} Price`,
                        data: prices,
                        borderColor: '#F0B90B',
                        backgroundColor: 'rgba(240, 185, 11, 0.2)',
                        fill: true,
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'top',
                        },
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Date'
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'Price (USD)'
                            }
                        }
                    }
                }
            });

            document.getElementById('graphModal').style.display = 'flex';
        }

        function debounce(func, wait) {
            let timeout;
            return function (...args) {
                clearTimeout(timeout);
                timeout = setTimeout(() => func.apply(this, args), wait);
            };
        }

        document.querySelector('.search-bar').addEventListener('input', debounce((e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filteredData = marketDataCache.filter(coin => {
                const asset = assets.find(a => a.id === coin.id);
                return asset.name.toLowerCase().includes(searchTerm) || asset.symbol.toLowerCase().includes(searchTerm);
            });
            updateMarketList(filteredData);
        }, 300));

        document.getElementById('market-list').addEventListener('click', (e) => {
            const row = e.target.closest('.table-row');
            if (row) {
                showGraph(row.dataset.coinId);
            }
        });

        document.querySelector('.close-btn').addEventListener('click', () => {
            document.getElementById('graphModal').style.display = 'none';
        });

        fetchMarketData();
        setInterval(fetchMarketData, 10000);
    </script>
</body>
</html>
