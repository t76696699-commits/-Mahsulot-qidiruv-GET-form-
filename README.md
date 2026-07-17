1. config.js (Sozlamalar)
Tokenni shu yerda saqlaysiz.

JavaScript
export const CONFIG = {
  GITHUB_TOKEN: 'YOUR_GITHUB_TOKEN', // GitHub Personal Access Token
  BASE_URL: 'https://api.github.com'
};
2. api.js (Universal Wrapper)
AbortController (timeout uchun) va retry mexanizmini o‘z ichiga oladi.

JavaScript
import { CONFIG } from './config.js';

export async function api(url, options = {}, retries = 3, timeout = 5000) {
  const controller = new AbortController();
  const id = setTimeout(() => controller.abort(), timeout);

  try {
    const response = await fetch(`${CONFIG.BASE_URL}${url}`, {
      ...options,
      headers: {
        'Authorization': `token ${CONFIG.GITHUB_TOKEN}`,
        'Content-Type': 'application/json',
        ...options.headers
      },
      signal: controller.signal
    });

    clearTimeout(id);
    
    // Rate limit tahlili
    const remaining = response.headers.get('x-ratelimit-remaining');
    console.log(`[Rate Limit] Qolgan so'rovlar: ${remaining}`);

    if (!response.ok) throw new Error(`HTTP Error: ${response.status}`);
    return await response.json();
  } catch (error) {
    if (retries > 0) return api(url, options, retries - 1, timeout);
    throw error;
  }
}
3. githubService.js (Async Generator)
Pagination bilan ishlash uchun.

JavaScript
import { api } from './api.js';

export async function* getUserRepos(username) {
  let page = 1;
  while (true) {
    const repos = await api(`/users/${username}/repos?per_page=100&page=${page}`);
    if (repos.length === 0) break;
    yield repos;
    page++;
  }
}
4. main.js (Asosiy mantiq)
Natijani console.table orqali chiqarish.

JavaScript
import { api } from './api.js';
import { getUserRepos } from './githubService.js';

async function run(username) {
  try {
    const user = await api(`/users/${username}`);
    console.log(`Foydalanuvchi: ${user.login} (${user.name})`);

    const tableData = [];
    for await (const page of getUserRepos(username)) {
      page.forEach(repo => {
        tableData.push({
          Name: repo.name,
          Stars: repo.stargazers_count,
          Forks: repo.forks_count,
          Language: repo.language || 'N/A'
        });
      });
    }
    console.table(tableData);
  } catch (err) {
    console.error("Xatolik:", err.message);
  }
}

run('uz-dev'); // O'zingizni GitHub usernameni yozing
