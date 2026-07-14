<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zamonaviy Admin Dashboard</title>
    <style>
        /* 6. CSS Custom Properties (Variables) */
        :root {
            --primary-color: #4f46e5;
            --primary-light: #e0e7ff;
            --success-color: #10b981;
            --success-bg: #d1fae5;
            --warning-color: #f59e0b;
            --warning-bg: #fef3c7;
            --danger-color: #ef4444;
            --danger-bg: #fee2e2;
            --dark-bg: #0f172a;
            --sidebar-width: 260px;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --body-bg: #f8fafc;
            --white: #ffffff;
            --border-color: #e2e8f0;
            --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
            --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
            --transition: all 0.3s ease;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--body-bg);
            color: var(--text-main);
            overflow-x: hidden;
        }

        /* 5. Grid bilan 2 ustunli layout (Sidebar + Main) */
        .dashboard-container {
            display: grid;
            grid-template-columns: var(--sidebar-width) 1fr;
            min-height: 100vh;
        }

        /* 1. Sticky Sidebar */
        aside.sidebar {
            background-color: var(--dark-bg);
            color: var(--white);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            height: 100vh;
            position: sticky;
            top: 0;
            padding: 24px;
            z-index: 100;
        }

        .logo-section {
            font-size: 1.5rem;
            font-weight: 800;
            color: var(--white);
            margin-bottom: 40px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo-dot {
            width: 12px;
            height: 12px;
            background-color: var(--primary-color);
            border-radius: 50%;
        }

        .nav-links {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 8px;
            flex-grow: 1;
        }

        .nav-links a {
            display: flex;
            align-items: center;
            gap: 12px;
            color: #94a3b8;
            text-decoration: none;
            padding: 12px 16px;
            border-radius: 8px;
            font-weight: 500;
            transition: var(--transition);
        }

        .nav-links li.active a,
        .nav-links a:hover {
            color: var(--white);
            background-color: rgba(255, 255, 255, 0.08);
        }

        .nav-links li.active a {
            background-color: var(--primary-color);
        }

        /* Foydalanuvchi profili (Sidebar pastida) */
        .user-profile {
            display: flex;
            align-items: center;
            gap: 12px;
            border-top: 1px solid #1e293b;
            padding-top: 20px;
        }

        .profile-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: var(--primary-color);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: var(--white);
        }

        .profile-info h4 {
            font-size: 0.9rem;
            font-weight: 600;
        }

        .profile-info p {
            font-size: 0.75rem;
            color: #64748b;
        }

        /* Asosiy kontent maydoni */
        main.main-content {
            padding: 30px;
            display: flex;
            flex-direction: column;
            gap: 30px;
            overflow-y: auto;
        }

        /* 2. Top Header */
        header.top-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: var(--white);
            padding: 16px 24px;
            border-radius: 12px;
            box-shadow: var(--shadow-sm);
            border: 1px solid var(--border-color);
        }

        .page-title h1 {
            font-size: 1.5rem;
            font-weight: 700;
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .search-box {
            position: relative;
        }

        .search-box input {
            padding: 10px 16px 10px 36px;
            border-radius: 8px;
            border: 1px solid var(--border-color);
            outline: none;
            width: 240px;
            font-size: 0.9rem;
            transition: var(--transition);
        }

        .search-box input:focus {
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.15);
        }

        .search-icon {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-muted);
        }

        .notifications-btn {
            background: none;
            border: none;
            font-size: 1.25rem;
            cursor: pointer;
            position: relative;
            padding: 5px;
        }

        .notification-badge {
            position: absolute;
            top: 2px;
            right: 2px;
            width: 8px;
            height: 8px;
            background-color: var(--danger-color);
            border-radius: 50%;
        }

        /* 3. Stats Kartalar (Grid bilan kamida 4 ta ko'rsatkich) */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
        }

        .stat-card {
            background-color: var(--white);
            padding: 24px;
            border-radius: 12px;
            box-shadow: var(--shadow-sm);
            border: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            transition: var(--transition);
        }

        .stat-card:hover {
            transform: translateY(-3px);
            box-shadow: var(--shadow-md);
        }

        .stat-title {
            font-size: 0.85rem;
            color: var(--text-muted);
            text-transform: uppercase;
            font-weight: 600;
            letter-spacing: 0.05em;
            margin-bottom: 10px;
        }

        .stat-value {
            font-size: 1.75rem;
            font-weight: 700;
            color: var(--text-main);
            margin-bottom: 8px;
        }

        .stat-trend {
            font-size: 0.8rem;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 4px;
        }

        .trend-up { color: var(--success-color); }
        .trend-down { color: var(--danger-color); }

        .stat-icon {
            width: 48px;
            height: 48px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            background-color: var(--primary-light);
            color: var(--primary-color);
        }

        /* 4. Data Table */
        .table-section {
            background-color: var(--white);
            border-radius: 12px;
            box-shadow: var(--shadow-sm);
            border: 1px solid var(--border-color);
            overflow: hidden;
        }

        .table-header {
            padding: 20px 24px;
            border-bottom: 1px solid var(--border-color);
            font-weight: 700;
            font-size: 1.1rem;
        }

        .table-responsive {
            width: 100%;
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        th {
            background-color: var(--body-bg);
            color: var(--text-muted);
            font-weight: 600;
            font-size: 0.85rem;
            text-transform: uppercase;
            padding: 14px 24px;
            border-bottom: 1px solid var(--border-color);
        }

        td {
            padding: 16px 24px;
            border-bottom: 1px solid var(--border-color);
            font-size: 0.9rem;
        }

        tr:last-child td {
            border-bottom: none;
        }

        tr:hover td {
            background-color: var(--body-bg);
        }

        /* Status badges */
        .badge {
            display: inline-flex;
            padding: 4px 10px;
            border-radius: 50px;
            font-size: 0.75rem;
            font-weight: 600;
        }

        .badge-success { background-color: var(--success-bg); color: var(--success-color); }
        .badge-pending { background-color: var(--warning-bg); color: var(--warning-color); }
        .badge-failed { background-color: var(--danger-bg); color: var(--danger-color); }

        /* 6. Responsive - Mobilgacha yiqiladigan sidebar */
        @media (max-width: 992px) {
            .dashboard-container {
                grid-template-columns: 1fr; /* Sidebar alohida qatorga o'tadi yoki yashirinadi */
            }

            aside.sidebar {
                height: auto;
                position: relative;
                padding: 16px 20px;
            }

            .logo-section {
                margin-bottom: 20px;
            }

            .nav-links {
                flex-direction: row;
                flex-wrap: wrap;
                gap: 10px;
                margin-bottom: 20px;
            }

            .nav-links a {
                padding: 8px 12px;
            }

            .user-profile {
                border-top: none;
                padding-top: 0;
                justify-content: flex-start;
            }
        }

        @media (max-width: 600px) {
            header.top-header {
                flex-direction: column;
                gap: 15px;
                align-items: flex-start;
            }

            .search-box {
                width: 100%;
            }

            .search-box input {
                width: 100%;
            }

            .header-actions {
                width: 100%;
                justify-content: space-between;
            }

            main.main-content {
                padding: 15px;
            }
        }
    </style>
</head>
<body>

    <div class="dashboard-container">
        <!-- 1. Sidebar -->
        <aside class="sidebar">
            <div>
                <div class="logo-section">
                    <div class="logo-dot"></div>
                    <span>ApexAdmin</span>
                </div>
                <ul class="nav-links">
                    <li class="active"><a href="#">📊 Asosiy panel</a></li>
                    <li><a href="#">📦 Buyurtmalar</a></li>
                    <li><a href="#">👥 Mijozlar</a></li>
                    <li><a href="#">⚙️ Sozlamalar</a></li>
                </ul>
            </div>
            
            <!-- Foydalanuvchi profili -->
            <div class="user-profile">
                <div class="profile-avatar">SJ</div>
                <div class="profile-info">
                    <h4>Sardor Jamilov</h4>
                    <p>Bosh administrator</p>
                </div>
            </div>
        </aside>

        <!-- Asosiy ishchi maydon -->
        <main class="main-content">
            <!-- 2. Top Header -->
            <header class="top-header">
                <div class="page-title">
                    <h1>Boshqaruv paneli</h1>
                </div>
                <div class="header-actions">
                    <div class="search-box">
                        <span class="search-icon">🔍</span>
                        <input type="text" placeholder="Qidirish...">
                    </div>
                    <button class="notifications-btn">
                        🔔
                        <span class="notification-badge"></span>
                    </button>
                </div>
            </header>

            <!-- 3. Stats Kartalari (4 ta ko'rsatkich) -->
            <section class="stats-grid">
                <!-- 1-karta: Foydalanuvchilar -->
                <div class="stat-card">
                    <div>
                        <p class="stat-title">Foydalanuvchilar</p>
                        <h3 class="stat-value">12,450</h3>
                        <span class="stat-trend trend-up">▲ 12.5% o'sish</span>
                    </div>
                    <div class="stat-icon">👥</div>
                </div>

                <!-- 2-karta: Daromad -->
                <div class="stat-card">
                    <div>
                        <p class="stat-title">Oylik Daromad</p>
                        <h3 class="stat-value">$45,230</h3>
                        <span class="stat-trend trend-up">▲ 8.3% o'sish</span>
                    </div>
                    <div class="stat-icon" style="background-color: #d1fae5; color: var(--success-color);">💵</div>
                </div>

                <!-- 3-karta: Buyurtmalar -->
                <div class="stat-card">
                    <div>
                        <p class="stat-title">Buyurtmalar</p>
                        <h3 class="stat-value">1,120</h3>
                        <span class="stat-trend trend-down">▼ 2.1% pasayish</span>
                    </div>
                    <div class="stat-icon" style="background-color: #fef3c7; color: var(--warning-color);">📦</div>
                </div>

                <!-- 4-karta: Konversiya (O'sish sur'ati) -->
                <div class="stat-card">
                    <div>
                        <p class="stat-title">Konversiya</p>
                        <h3 class="stat-value">4.85%</h3>
                        <span class="stat-trend trend-up">▲ 1.2% o'sish</span>
                    </div>
                    <div class="stat-icon" style="background-color: #fee2e2; color: #ef4444;">📈</div>
                </div>
            </section>

            <!-- 4. Data Table (Ma'lumotlar jadvali) -->
            <section class="table-section">
                <div class="table-header">
                    Oxirgi buyurtmalar
                </div>
                <div class="table-responsive">
                    <table>
                        <thead>
                            <tr>
                                <th>Buyurtma ID</th>
                                <th>Mijoz</th>
                                <th>Sana</th>
                                <th>Summa</th>
                                <th>Status</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>#ORD-8493</td>
                                <td>Alisher Qodirov</td>
                                <td>14 Iyul, 2026</td>
                                <td>$120.00</td>
                                <td><span class="badge badge-success">Yetkazildi</span></td>
                            </tr>
                            <tr>
                                <td>#ORD-8492</td>
                                <td>Kamola Aliyeva</td>
                                <td>14 Iyul, 2026</td>
                                <td>$340.50</td>
                                <td><span class="badge badge-pending">Kutilmoqda</span></td>
                            </tr>
                            <tr>
                                <td>#ORD-8491</td>
                                <td>Bekzod Rahimov</td>
                                <td>13 Iyul, 2026</td>
                                <td>$85.00</td>
                                <td><span class="badge badge-success">Yetkazildi</span></td>
                            </tr>
                            <tr>
                                <td>#ORD-8490</td>
                                <td>Zuhra Karimoova</td>
                                <td>12 Iyul, 2026</td>
                                <td>$450.00</td>
                                <td><span class="badge badge-failed">Bekor qilindi</span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </section>
        </main>
    </div>

</body>
</html>
