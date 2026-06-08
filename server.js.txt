const express = require('express');
const axios = require('axios');
const cheerio = require('cheerio');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/api/version', async (req, res) => {
    try {
        const response = await axios.get('https://one-piece-bounty-rush.en.uptodown.com/android/versions', {
            headers: { 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)' }
        });
        const $ = cheerio.load(response.data);
        
        let targetVersion = "92000"; 
        $('div').each((i, el) => {
            const text = $(el).text().trim();
            if (/^v?\d+(\.\d+)*$/.test(text) || (text.length === 5 && /^\d+$/.test(text))) {
                targetVersion = text.replace('v', '').trim();
                return false; 
            }
        });
        });

    } catch (error) {
        res.status(500).json({ status: "error", message: error.message });
    }
});

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
