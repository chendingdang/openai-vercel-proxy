const axios = require('axios');

module.exports = async (req, res) => {
  const { message } = req.body;

  try {
    const response = await axios.post(
      'https://api.openai.com/v1/chat/completions',
      {
        model: 'gpt-3.5-turbo',
        messages: [{ role: 'user', content: message }],
      },
      {
        headers: {
          'Authorization': `Bearer ${process.env.OPENAI_API_KEY}`,
          'Content-Type': 'application/json',
        },
      }
    );

    res.status(200).json(response.data);
  } catch (error) {
    console.error('OpenAI API 错误:', error.response?.data || error.message);
    res.status(500).json({ error: 'OpenAI 请求失败' });
  }
};
