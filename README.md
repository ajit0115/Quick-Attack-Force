<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quick Attack Force - National Portal</title>
    <style>
        /* CSS (largely the same as the previous version, with minor additions for user table) */
        :root {
            --primary-color: #003366; /* Deep Navy Blue */
            --secondary-color: #004080; /* Slightly Lighter Blue */
            --accent-color: #FF9933; /* Saffron/Orange */
            --highlight-color: #FFD700; /* Gold */
            --text-color: #EAEAEA;
            --text-dark: #212529;
            --bg-light: #f8f9fa;
            --border-color-input: #ced4da; /* Standard input border */
            --border-color-strong: #0059b3;
            --header-height: 80px;
            --sidebar-width: 260px;
            --font-family: 'Roboto', 'Segoe UI', Arial, sans-serif;
            --police-khaki: #C3B091;
            --police-official-blue: #002244;
            --police-button-khaki-bg: #A08D78;
            --police-button-khaki-text: #FFFFFF;
            --focus-ring-color: rgba(255, 153, 51, 0.5); /* Accent color with alpha for focus */
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: var(--font-family); background-color: var(--bg-light); color: var(--text-dark); line-height: 1.6; display: flex; flex-direction: column; min-height: 100vh; }
        .profile-placeholder-svg { width: 50px; height: 50px; border-radius: 50%; background-color: #B0C4DE; display: inline-block; vertical-align: middle; }
        .profile-placeholder-svg svg { width: 100%; height: 100%; fill: var(--secondary-color); }
        .indian-flag-svg { width: 45px; height: auto; margin-right: 10px; vertical-align: middle; }
        .app-header { background-color: var(--primary-color); color: var(--text-color); height: var(--header-height); display: flex; align-items: center; justify-content: space-between; padding: 0 25px; position: fixed; top: 0; left: 0; right: 0; z-index: 1000; box-shadow: 0 4px 8px rgba(0,0,0,0.15); }
        .app-header .logo-area { display: flex; align-items: center; }
        .app-header .logo-area h1 { font-size: 1.6em; font-weight: bold; color: var(--highlight-color); margin-left: 10px; }
        .app-header .header-actions { display: flex; align-items: center; }
        .app-header .search-bar input { padding: 9px 15px; border-radius: 25px; border: 1px solid var(--border-color-strong); background-color: var(--secondary-color); color: var(--text-color); width: 220px; font-size: 0.9em; transition: box-shadow 0.2s; }
        .app-header .search-bar input::placeholder { color: #bdc3c7; }
        .app-header .search-bar input:focus { box-shadow: 0 0 0 0.2rem var(--focus-ring-color); outline: none; }
        .header-button { background-color: var(--accent-color); color: var(--bg-light); border: none; font-size: 0.9em; font-weight: bold; cursor: pointer; margin-left: 10px; padding: 8px 15px; border-radius: 5px; transition: background-color 0.3s, box-shadow 0.2s; }
        .header-button:hover { background-color: color-mix(in srgb, var(--accent-color) 80%, black); }
        .header-button:focus { box-shadow: 0 0 0 0.2rem var(--focus-ring-color); outline: none; }
        .header-button.user-login { background-color: var(--secondary-color); border: 1px solid var(--accent-color);}
        .app-container { display: flex; flex-grow: 1; padding-top: var(--header-height); }
        .app-sidebar { width: var(--sidebar-width); background-color: var(--secondary-color); padding-top: 25px; height: calc(100vh - var(--header-height)); position: fixed; left: 0; overflow-y: auto; box-shadow: 2px 0 5px rgba(0,0,0,0.1); }
        .app-sidebar nav ul { list-style: none; }
        .app-sidebar nav ul li a { display: block; padding: 13px 25px; color: var(--text-color); text-decoration: none; transition: background-color 0.2s, color 0.2s; border-left: 4px solid transparent; font-size: 0.95em; }
        .app-sidebar nav ul li a:hover, .app-sidebar nav ul li a.active { background-color: var(--primary-color); color: var(--highlight-color); border-left-color: var(--accent-color); }
        .app-sidebar nav ul li a i { margin-right: 12px; width: 20px; text-align: center; }
        .app-sidebar .admin-link a { background-color: var(--police-official-blue); border-left-color: var(--police-khaki); }
        .app-sidebar .admin-link a:hover { background-color: color-mix(in srgb, var(--police-official-blue) 80%, black); }
        .app-main-content { flex-grow: 1; padding: 30px; margin-left: var(--sidebar-width); background-color: #eef2f7; color: var(--text-dark); }
        .content-section { display: none; }
        .content-section.active { display: block; }
        .content-section > h1, .content-section > h2 { color: var(--primary-color); margin-bottom: 25px; border-bottom: 2px solid var(--accent-color); padding-bottom: 12px; font-weight: 600; }
        .content-section > h2 { font-size: 1.5em; margin-top:30px; }
        .police-section-header { background-color: var(--police-official-blue); color: var(--text-color); padding: 15px 20px; margin: -30px -30px 25px -30px; border-bottom: 3px solid var(--police-khaki); }
        .police-section-header h1, .police-section-header h2 { color: var(--text-color) !important; border-bottom: none !important; margin-bottom: 0 !important; padding-bottom: 0 !important; }
        .hero-banner { background-color: var(--secondary-color); color: var(--text-color); padding: 40px; border-radius: 8px; margin-bottom: 30px; text-align: center; background-image: linear-gradient(rgba(0, 51, 102, 0.75), rgba(0, 51, 102, 0.75)), url('https://via.placeholder.com/1200x300/003366/FFFFFF?text=Quick+Attack+Force+Portal'); background-size: cover; background-position: center; }
        .hero-banner h1 { color: var(--highlight-color); border-bottom: none; font-size: 2.5em; margin-bottom: 10px;}
        .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 25px; }
        .dashboard-card { background-color: var(--bg-light); padding: 25px; border-radius: 8px; box-shadow: 0 3px 10px rgba(0,0,0,0.08); border-left: 5px solid var(--accent-color); }
        .dashboard-card h3 { color: var(--primary-color); margin-bottom: 15px; font-size: 1.2em; }
        .dashboard-card .stat-number { font-size: 2.2em; font-weight: bold; color: var(--primary-color); margin-bottom: 8px; }
        .dashboard-card ul { list-style: none; padding: 0; }
        .dashboard-card ul li a { color: var(--secondary-color); text-decoration: none; display: block; padding: 6px 0; transition: color 0.2s; }
        .dashboard-card ul li a:hover { color: var(--accent-color); }
        .data-table { width: 100%; border-collapse: collapse; margin-bottom: 25px; background-color: var(--bg-light); box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-radius: 6px; overflow: hidden; }
        .data-table th, .data-table td { border-bottom: 1px solid #dee2e6; padding: 12px 15px; text-align: left; vertical-align: middle; }
        .data-table th { background-color: #e9ecef; color: var(--primary-color); font-weight: 600; text-transform: uppercase; font-size: 0.85em; }
        .police-table thead th { background-color: var(--police-official-blue) !important; color: var(--police-khaki) !important; border-bottom: 2px solid var(--police-khaki) !important; }
        .data-table tr:hover { background-color: #f1f3f5; }
        .data-table .profile-img-cell { width: 70px; text-align: center; }
        .data-table .profile-img-cell .profile-placeholder-svg, .data-table .profile-img-cell img { width: 45px; height: 45px; border-radius:50%; object-fit:cover; }
        .data-table .actions-cell button { margin-right: 5px; padding: 6px 10px; font-size: 0.85em; }
        .action-button { background-color: var(--accent-color); color: var(--bg-light); padding: 8px 15px; border: none; border-radius: 5px; cursor: pointer; font-size: 0.9em; font-weight: 500; transition: background-color 0.3s, box-shadow 0.2s; margin-top: 10px; }
        .action-button:hover { background-color: color-mix(in srgb, var(--accent-color) 80%, black); }
        .action-button:focus { box-shadow: 0 0 0 0.2rem var(--focus-ring-color); outline: none; }
        .action-button.secondary { background-color: var(--secondary-color); }
        .action-button.secondary:hover { background-color: var(--primary-color); }
        .action-button.police-khaki { background-color: var(--police-button-khaki-bg); color: var(--police-button-khaki-text); border: 1px solid color-mix(in srgb, var(--police-button-khaki-bg) 70%, black); }
        .action-button.police-khaki:hover { background-color: color-mix(in srgb, var(--police-button-khaki-bg) 85%, black); }
        .modal { display: none; position: fixed; z-index: 1001; left: 0; top: 0; width: 100%; height: 100%; overflow: auto; background-color: rgba(0,0,0,0.55); align-items: center; justify-content: center; animation: fadeIn 0.3s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
        .modal-content { background-color: var(--bg-light); color: var(--text-dark); margin: auto; padding: 30px 35px; border-top: 5px solid var(--accent-color); width: 90%; max-width: 550px; border-radius: 8px; box-shadow: 0 5px 15px rgba(0,0,0,0.2); position: relative; }
        .modal-content h2 { color: var(--primary-color); margin-bottom: 25px; border-bottom:none; padding-bottom:0; font-size: 1.4em;}
        .modal-content label { display: block; margin-bottom: 6px; font-weight: 500; font-size: 0.9em; }
        .modal-content input[type="text"], .modal-content input[type="password"], .modal-content input[type="email"], .modal-content input[type="file"], .modal-content select, .modal-content textarea { width: 100%; padding: 10px 12px; margin-bottom: 18px; border: 1px solid var(--border-color-input); border-radius: 4px; font-size: 0.95em; transition: border-color 0.2s, box-shadow 0.2s; }
        .modal-content input:focus, .modal-content select:focus, .modal-content textarea:focus { border-color: var(--accent-color); box-shadow: 0 0 0 0.2rem var(--focus-ring-color); outline: none;}
        .modal-content button[type="submit"] { background-color: var(--accent-color); color: var(--bg-light); padding: 12px 25px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; font-size: 1em; width: 100%; transition: background-color 0.3s, box-shadow 0.2s; }
        .modal-content button[type="submit"]:hover { background-color: color-mix(in srgb, var(--accent-color) 80%, black); }
        .modal-content button[type="submit"]:focus { box-shadow: 0 0 0 0.2rem var(--focus-ring-color); outline: none; }
        .close-btn { color: #888; position: absolute; top: 15px; right: 20px; font-size: 30px; font-weight: bold; cursor: pointer; }
        .close-btn:hover { color: var(--text-dark); }
        .admin-card { background-color: var(--bg-light); padding: 20px; border-radius: 6px; box-shadow: 0 2px 6px rgba(0,0,0,0.07); margin-bottom: 20px; border-left: 4px solid var(--police-official-blue); }
        .admin-card h3 { color: var(--police-official-blue); margin-bottom: 15px; font-size: 1.2em;}
        .app-footer { background-color: var(--primary-color); color: #bdc3c7; text-align: center; padding: 25px; font-size: 0.9em; border-top: 3px solid var(--accent-color); }
        .app-footer p { margin: 6px 0; }
        .app-footer a { color: var(--highlight-color); text-decoration: none; }
        .app-footer a:hover { text-decoration: underline; }
        @media (max-width: 992px) { .app-sidebar { width: 0; } .app-main-content { margin-left: 0; padding: 15px;} .app-header .search-bar { display: none; } .police-section-header { margin-left: -15px; margin-right: -15px; padding: 15px;} }
        .list-item { background: #fff; padding: 15px; margin-bottom: 10px; border-radius: 5px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); display: flex; justify-content: space-between; align-items: center; }
        .list-item .action-button { margin-top: 0; }
        .image-preview { max-width: 100px; max-height: 100px; margin-top: 10px; border: 1px solid #ddd; padding: 5px; display: none; }
        .image-preview.profile { border-radius: 50%; width: 80px; height: 80px; object-fit: cover; }

    </style>
</head>
<body>
    <!-- SVG Definitions (same) -->
    <svg style="display:none;">
      <symbol id="profile-avatar" viewBox="0 0 100 100"><circle cx="50" cy="35" r="20" fill="currentColor"/><path d="M30 70 Q50 50 70 70 L70 90 Q50 100 30 90 Z" fill="currentColor"/></symbol>
      <symbol id="indian-flag" viewBox="0 0 900 600"><rect width="900" height="600" fill="#FFF"/><rect width="900" height="200" fill="#FF9933"/><rect y="400" width="900" height="200" fill="#138808"/><circle cx="450" cy="300" r="80" fill="none" stroke="#000080" stroke-width="15"/><path d="M450,300 L450,220 M450,300 L450,380 M450,300 L370,300 M450,300 L530,300 M450,300 L406,239 M450,300 L494,361 M450,300 L389,265 M450,300 L511,335 M450,300 L389,335 M450,300 L511,265 M450,300 L406,361 M450,300 L494,239" stroke="#000080" stroke-width="10" stroke-linecap="round"/><circle cx="450" cy="300" r="20" fill="#000080"/></symbol>
    </svg>

    <header class="app-header"> <!-- Header (same) -->
        <div class="logo-area"><svg class="indian-flag-svg"><use xlink:href="#indian-flag"/></svg><h1>Quick Attack Force</h1></div>
        <div class="header-actions"><div class="search-bar"><input type="text" placeholder="Search Portal..." aria-label="Search"></div><button class="header-button user-login" onclick="openModal('userLoginModal')">User Login</button><button class="header-button admin-login" onclick="openModal('adminLoginModal')">Admin Login</button></div>
    </header>

    <div class="app-container">
        <aside class="app-sidebar"> <!-- Sidebar (same) -->
            <nav><ul><li><a href="#" class="nav-link active" onclick="showSection('dashboard', this)"><i>🏠</i> Dashboard</a></li><li><a href="#" class="nav-link" onclick="showSection('branches', this)"><i>🏛️</i> Branches & Personnel</a></li><li><a href="#" class="nav-link" onclick="showSection('directory', this)"><i>👥</i> Central Directory</a></li><li><a href="#" class="nav-link" onclick="showSection('documents', this)"><i>📁</i> Secure Documents</a></li><li><a href="#" class="nav-link" onclick="showSection('gallery', this)"><i>🖼️</i> Image Gallery</a></li><li><a href="#" class="nav-link" onclick="showSection('notifications', this)"><i>🔔</i> Alerts & Notifications</a></li><li class="admin-link"><a href="#" class="nav-link" onclick="showSection('adminPanel', this)"><i>🛡️</i> Admin Control Panel</a></li><li><a href="#" class="nav-link" onclick="showSection('settings', this)"><i>⚙️</i> My Settings</a></li></ul></nav>
        </aside>

        <main class="app-main-content">
            <!-- Dashboard Section (same) -->
            <section id="dashboard" class="content-section active"><div class="hero-banner"><h1>Welcome to Quick Attack Force Portal</h1><p>सतर्कता, सुरक्षा, सेवा (Vigilance, Security, Service)</p></div><div class="dashboard-grid"><div class="dashboard-card"><h3>Active Operations</h3><p class="stat-number">18</p><p>3 Critical, 15 Standard</p></div><div class="dashboard-card"><h3>Personnel On-Duty</h3><p class="stat-number">875</p><p>System Wide Count</p></div><div class="dashboard-card"><h3>Recent Intel Briefs</h3><p class="stat-number">5</p><p>Last brief: 2h ago</p></div><div class="dashboard-card quick-links"><h3>Quick Access</h3><ul><li><a href="#" onclick="showSection('branches');">View Branch Details</a></li><li><a href="#" onclick="showSection('directory');">Personnel Search</a></li><li><a href="#" onclick="showSection('documents');">SOP & Guidelines</a></li><li><a href="#" onclick="openModal('submitReportModal');">Submit Incident Report</a></li></ul></div></div></section>

            <!-- Branches & Personnel Section (structure same, JS interaction updated) -->
            <section id="branches" class="content-section">
                <div class="police-section-header"><h1>Branches & Personnel Management</h1></div>
                <div class="branch-unit">
                    <h2>Data & Analytics Unit</h2>
                    <button class="action-button police-khaki" onclick="openAddEmployeeModal('Data')">Add Employee (Data)</button>
                    <table class="data-table police-table" style="margin-top:15px;">
                        <thead><tr><th>Photo</th><th>ID</th><th>Name</th><th>Designation</th><th>Contact</th><th class="actions-cell">Actions</th></tr></thead>
                        <tbody id="dataBranchTableBody">
                            <tr data-id="DT-001" data-photo-src="">
                                <td class="profile-img-cell"><div class="profile-placeholder-svg"><svg><use xlink:href="#profile-avatar"/></svg></div></td>
                                <td>DT-001</td><td>Dr. Evelyn Reed</td><td>Lead Data Scientist</td><td>ereed@qaf.gov</td>
                                <td class="actions-cell"><button class="action-button secondary" onclick="openEditEmployeeModal(this.closest('tr'), 'Data')">Edit</button></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                 <div class="branch-unit" style="margin-top: 30px;">
                    <h2>Cyber Crime (CC) Unit</h2>
                    <button class="action-button police-khaki" onclick="openAddEmployeeModal('CC')">Add Employee (CC)</button>
                    <table class="data-table police-table" style="margin-top:15px;">
                         <thead><tr><th>Photo</th><th>ID</th><th>Name</th><th>Designation</th><th>Contact</th><th class="actions-cell">Actions</th></tr></thead>
                        <tbody id="ccBranchTableBody">
                             <tr data-id="CC-001" data-photo-src="">
                                <td class="profile-img-cell"><div class="profile-placeholder-svg"><svg><use xlink:href="#profile-avatar"/></svg></div></td>
                                <td>CC-001</td><td>Marcus Chen</td><td>Cybersecurity Head</td><td>mchen@qaf.gov</td>
                                <td class="actions-cell"><button class="action-button secondary" onclick="openEditEmployeeModal(this.closest('tr'), 'CC')">Edit</button></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- Central Directory Section (structure same, JS interaction updated) -->
            <section id="directory" class="content-section">
                <div class="police-section-header"><h1>Central Personnel Directory</h1></div>
                <button class="action-button police-khaki" onclick="openAddEmployeeModal('Central Directory')" style="margin-bottom: 15px;">Add Employee to Directory</button>
                <input type="text" placeholder="Search by Name, ID, Branch, Designation..." style="width: 100%; padding: 12px; margin-bottom: 25px; border: 1px solid var(--border-color-input); border-radius: 5px; font-size: 1em;">
                <table class="data-table police-table">
                    <thead><tr><th>Photo</th><th>ID</th><th>Name</th><th>Designation</th><th>Branch</th><th>Contact</th><th class="actions-cell">Actions</th></tr></thead>
                    <tbody id="centralDirectoryTableBody">
                       <tr data-id="DT-001_central" data-photo-src=""><td class="profile-img-cell"><div class="profile-placeholder-svg"><svg><use xlink:href="#profile-avatar"/></svg></div></td><td>DT-001</td><td>Dr. Evelyn Reed</td><td>Lead Data Scientist</td><td>Data</td><td>ereed@qaf.gov</td><td class="actions-cell"><button class="action-button secondary" onclick="openEditEmployeeModal(this.closest('tr'), 'Central Directory')">Edit</button></td></tr>
                       <tr data-id="CC-001_central" data-photo-src=""><td class="profile-img-cell"><div class="profile-placeholder-svg"><svg><use xlink:href="#profile-avatar"/></svg></div></td><td>CC-001</td><td>Marcus Chen</td><td>Cybersecurity Head</td><td>Cyber Crime</td><td>mchen@qaf.gov</td><td class="actions-cell"><button class="action-button secondary" onclick="openEditEmployeeModal(this.closest('tr'), 'Central Directory')">Edit</button></td></tr>
                    </tbody>
               </table>
            </section>

            <!-- Other Sections (Documents, Gallery, Notifications, Admin, Settings - largely same structure) -->
            <section id="documents" class="content-section"><h1>Secure Document Repository</h1><button class="action-button" onclick="openModal('addDocumentLinkModal')" style="margin-bottom: 15px;">Add New Document</button><ul id="documentList" style="list-style: none; padding: 0;"><li class="list-item"><span>SOP_Evidence_Collection_v4.pdf</span><a href="#" download class="action-button secondary">Download</a></li></ul></section>
            <section id="gallery" class="content-section"><h1>Image Gallery</h1><button class="action-button" onclick="openModal('addImageToGalleryModal')" style="margin-bottom: 15px;">Add New Image</button><div class="image-gallery" id="imageGalleryContainer"><img src="https://via.placeholder.com/600x400/C3B091/FFFFFF?text=Hawa+Mahal+Khaki" alt="Hawa Mahal - View 1"><img src="https://via.placeholder.com/600x400/002244/FFFFFF?text=Hawa+Mahal+Police+Blue" alt="Hawa Mahal - View 2"></div></section>
            <section id="notifications" class="content-section"><h1>Alerts & Notifications</h1><button class="action-button" onclick="openModal('createNotificationModal')" style="margin-bottom: 15px;">Create New Notification</button><ul id="notificationListUL" style="list-style: none; padding: 0;"><li class="list-item" style="background: #fff1f0; border-left: 5px solid #ff4d4f;"><strong>URGENT:</strong> Unidentified drone activity Sector Gamma. (2 min ago)</li></ul></section>
            
            <section id="adminPanel" class="content-section">
                <div class="police-section-header"><h1>Administrator Control Panel</h1></div>
                <p style="margin-bottom: 20px;"><em>Note: This panel would typically be restricted to authorized administrators. All actions are logged.</em></p>
                
                <div class="admin-card">
                    <h3>Manage Portal Users</h3>
                    <p>Create and manage user accounts for portal access.</p>
                    <button class="action-button police-khaki" onclick="openModal('createUserModal')">Create New User</button>
                    <h4 style="margin-top: 20px; margin-bottom: 10px; color: var(--primary-color);">Existing Portal Users (Simulated)</h4>
                    <table class="data-table" id="portalUsersTable">
                        <thead>
                            <tr><th>Username</th><th>Full Name</th><th>Email</th><th>Role</th><th>Actions</th></tr>
                        </thead>
                        <tbody id="portalUsersTableBody">
                            <!-- Sample User (would be populated by JS in real app) -->
                            <tr>
                                <td>admin_qaf</td>
                                <td>Portal Administrator</td>
                                <td>admin@qaf.gov</td>
                                <td>Portal Admin</td>
                                <td class="actions-cell"><button class="action-button secondary" onclick="alert('Edit User (Simulated)')">Edit</button></td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <div class="admin-card"><h3>Manage Personnel</h3><p>Add, edit, or remove employee records across branches or centrally.</p><button class="action-button police-khaki" onclick="showSection('branches', document.querySelector('.app-sidebar a[onclick*=\'branches\']'))">Manage Branch Personnel</button><button class="action-button police-khaki" onclick="showSection('directory', document.querySelector('.app-sidebar a[onclick*=\'directory\']'))" style="margin-left:10px;">Manage Central Directory</button></div>
                <div class="admin-card"><h3>Manage Secure Documents (PDFs)</h3><p>Upload new official documents, SOPs, or guidelines.</p><button class="action-button police-khaki" onclick="openModal('adminUploadDocumentModal')">Upload New Document</button></div>
                <div class="admin-card"><h3>Manage Image Gallery</h3><p>Add or remove images from the public/internal gallery.</p><button class="action-button police-khaki" onclick="openModal('adminUploadImageModal')">Upload New Image</button></div>
                <div class="admin-card"><h3>Post Announcements/Alerts</h3><p>Create and publish new alerts or informational announcements for the portal.</p><button class="action-button police-khaki" onclick="openModal('adminPostAnnouncementModal')">Create New Announcement</button></div>
            </section>
            <section id="settings" class="content-section"><h1>My Settings (Simulated)</h1> <!-- Content same --></section>
        </main>
    </div>

    <footer class="app-footer"> <!-- Footer (same) -->
        <p>&copy; 2023 Quick Attack Force, Government of India. All Rights Reserved.</p><p>This portal is for authorized personnel only. Unauthorized access is strictly prohibited.</p><p><a href="#" onclick="alert('Contact: helpdesk@qaf.gov.in | Phone: 1800-XXX-XXXX')">Contact Us</a> | <a href="#" onclick="alert('Data Privacy Policy: Information on this portal is classified and protected under applicable national laws.')">Privacy Policy</a></p>
    </footer>

    <!-- Modals (User Login, Admin Login, Submit Report - same as before) -->
    <div id="userLoginModal" class="modal"> <div class="modal-content"><span class="close-btn" onclick="closeModal('userLoginModal')">&times;</span><h2>User Login</h2><form onsubmit="event.preventDefault(); alert('User Login Submitted (Simulated).'); closeModal('userLoginModal');"><div><label for="userUsername">Badge ID / Username:</label><input type="text" id="userUsername" required></div><div><label for="userPassword">Password:</label><input type="password" id="userPassword" required></div><button type="submit">Login</button></form></div></div>
    <div id="adminLoginModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('adminLoginModal')">&times;</span><h2>Administrator Login</h2><form onsubmit="event.preventDefault(); alert('Admin Login Submitted (Simulated).'); closeModal('adminLoginModal');"><div><label for="adminUsername">Admin ID:</label><input type="text" id="adminUsername" required></div><div><label for="adminPassword">Password:</label><input type="password" id="adminPassword" required></div><div><label for="adminOtp">Two-Factor Code:</label><input type="text" id="adminOtp" required></div><button type="submit" class="police-khaki">Secure Login</button></form></div></div>
    <div id="submitReportModal" class="modal"> <div class="modal-content"><span class="close-btn" onclick="closeModal('submitReportModal')">&times;</span><h2>Submit Incident Report</h2><form onsubmit="event.preventDefault(); alert('Report Submitted (Simulated).'); closeModal('submitReportModal');"><div><label for="reportTitle">Report Title:</label><input type="text" id="reportTitle" required></div><div><label for="reportDetails">Details:</label><textarea id="reportDetails" rows="5" style="width:100%; padding:10px; border:1px solid var(--border-color-input); border-radius:4px;" required></textarea></div><button type="submit">Submit Report</button></form></div></div>
    
    <!-- Add/Edit Employee Modal (Updated for File Input) -->
    <div id="employeeModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal('employeeModal')">&times;</span>
            <h2 id="employeeModalTitle">Employee Details</h2>
            <form id="employeeForm" onsubmit="event.preventDefault(); handleEmployeeFormSubmit();">
                <input type="hidden" id="employeeModalMode" value="add">
                <input type="hidden" id="employeeModalRowId" value="">
                <input type="hidden" id="employeeModalContext" value="">
                <div><label for="empName">Full Name:</label><input type="text" id="empName" required></div>
                <div><label for="empId">Employee ID:</label><input type="text" id="empId" required></div>
                <div><label for="empDesignation">Designation:</label><input type="text" id="empDesignation" required></div>
                <div id="empBranchFieldContainer" style="display:none;"><label for="empBranch">Branch:</label><input type="text" id="empBranch"></div>
                <div><label for="empContact">Contact Email:</label><input type="email" id="empContact" required></div>
                <div>
                    <label for="empPhotoFile">Photo (Optional):</label>
                    <input type="file" id="empPhotoFile" accept="image/*" onchange="previewImage(event, 'empPhotoPreview')">
                    <img id="empPhotoPreview" src="#" alt="Photo Preview" class="image-preview profile">
                </div>
                <button type="submit" id="employeeModalSubmitButton">Add Employee</button>
            </form>
        </div>
    </div>

    <!-- Admin Upload Modals (Updated for File Input where applicable) -->
    <div id="adminUploadDocumentModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('adminUploadDocumentModal')">&times;</span><h2>Upload New Document (PDF)</h2><form onsubmit="event.preventDefault(); alert('Document Upload Submitted (Admin Simulated). File: ' + document.getElementById('adminDocFile').files[0]?.name); closeModal('adminUploadDocumentModal');"><div><label for="adminDocTitle">Document Title:</label><input type="text" id="adminDocTitle" required></div><div><label for="adminDocFile">Select PDF File:</label><input type="file" id="adminDocFile" accept=".pdf" required></div><button type="submit" class="police-khaki">Upload Document</button></form></div></div>
    <div id="adminUploadImageModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal('adminUploadImageModal')">&times;</span>
            <h2>Upload New Image to Gallery</h2>
            <form onsubmit="event.preventDefault(); handleAdminUploadImage();">
                <div><label for="adminImageCaption">Image Caption:</label><input type="text" id="adminImageCaption" required></div>
                <div>
                    <label for="adminImageFileGallery">Select Image File:</label>
                    <input type="file" id="adminImageFileGallery" accept="image/*" onchange="previewImage(event, 'adminImagePreviewGallery')" required>
                    <img id="adminImagePreviewGallery" src="#" alt="Image Preview" class="image-preview">
                </div>
                <button type="submit" class="police-khaki">Upload Image</button>
            </form>
        </div>
    </div>
    <div id="adminPostAnnouncementModal" class="modal"> <!-- Admin Post Announcement (same) --> <div class="modal-content"><span class="close-btn" onclick="closeModal('adminPostAnnouncementModal')">&times;</span><h2>Post New Announcement/Alert</h2><form onsubmit="event.preventDefault(); alert('Announcement Posted (Admin Simulated). Title: ' + document.getElementById('adminAnnouncementTitle').value); closeModal('adminPostAnnouncementModal');"><div><label for="adminAnnouncementTitle">Title:</label><input type="text" id="adminAnnouncementTitle" required></div><div><label for="adminAnnouncementType">Type:</label><select id="adminAnnouncementType"><option value="info">Informational</option><option value="alert">Alert (Normal)</option><option value="urgent">Alert (Urgent)</option></select></div><div><label for="adminAnnouncementContent">Content:</label><textarea id="adminAnnouncementContent" rows="5" required></textarea></div><button type="submit" class="police-khaki">Post Announcement</button></form></div></div>
    
    <!-- Modals for Adding Content (Updated for File Input where applicable) -->
    <div id="addDocumentLinkModal" class="modal"> <!-- Add Document (same, assumes link) --> <div class="modal-content"><span class="close-btn" onclick="closeModal('addDocumentLinkModal')">&times;</span><h2>Add New Document Link</h2><form onsubmit="event.preventDefault(); handleAddDocumentLink();"><div><label for="docLinkTitle">Document Title:</label><input type="text" id="docLinkTitle" required></div><div><label for="docLinkUrl">Document URL:</label><input type="text" id="docLinkUrl" placeholder="e.g., https://example.com/doc.pdf" required></div><button type="submit">Add Document</button></form></div></div>
    <div id="addImageToGalleryModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal('addImageToGalleryModal')">&times;</span>
            <h2>Add Image to Gallery</h2>
            <form onsubmit="event.preventDefault(); handleAddImageToGallery();">
                <div><label for="galleryImageCaptionPublic">Caption:</label><input type="text" id="galleryImageCaptionPublic" required></div>
                <div>
                    <label for="galleryImageFilePublic">Select Image File:</label>
                    <input type="file" id="galleryImageFilePublic" accept="image/*" onchange="previewImage(event, 'galleryImagePreviewPublic')" required>
                     <img id="galleryImagePreviewPublic" src="#" alt="Image Preview" class="image-preview">
                </div>
                <button type="submit">Add Image</button>
            </form>
        </div>
    </div>
    <div id="createNotificationModal" class="modal"> <!-- Create Notification (same) --> <div class="modal-content"><span class="close-btn" onclick="closeModal('createNotificationModal')">&times;</span><h2>Create New Notification</h2><form onsubmit="event.preventDefault(); handleCreateNotification();"><div><label for="notificationText">Notification Text:</label><textarea id="notificationText" rows="3" required></textarea></div><div><label for="notificationType">Type:</label><select id="notificationType"><option value="info">Informational</option><option value="alert">Alert</option><option value="urgent">Urgent</option></select></div><button type="submit">Create Notification</button></form></div></div>

    <!-- Create User Modal (NEW) -->
    <div id="createUserModal" class="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal('createUserModal')">&times;</span>
            <h2>Create New Portal User</h2>
            <form id="createUserForm" onsubmit="event.preventDefault(); handleCreateUser();">
                <div><label for="newUserName">Username:</label><input type="text" id="newUserName" required></div>
                <div><label for="newUserFullName">Full Name:</label><input type="text" id="newUserFullName" required></div>
                <div><label for="newUserEmail">Email Address:</label><input type="email" id="newUserEmail" required></div>
                <div><label for="newUserRole">Role:</label>
                    <select id="newUserRole" required>
                        <option value="">-- Select Role --</option>
                        <option value="Standard User">Standard User</option>
                        <option value="Branch Admin">Branch Admin</option>
                        <option value="Portal Admin">Portal Admin</option>
                    </select>
                </div>
                <div><label for="newUserPassword">Initial Password:</label><input type="password" id="newUserPassword" required></div>
                <div><label for="newUserConfirmPassword">Confirm Password:</label><input type="password" id="newUserConfirmPassword" required></div>
                <button type="submit" class="police-khaki">Create User Account</button>
            </form>
        </div>
    </div>


    <script>
        // --- Utility and Core Functions ---
        function showSection(sectionId, navLink) { /* ... same ... */ }
        function openModal(modalId) { /* ... same ... */ }
        function closeModal(modalId) { /* ... same, with robust form reset ... */ }
        function previewImage(event, previewElementId) { /* ... same ... */ }

        // --- Employee Add/Edit Logic ---
        function openAddEmployeeModal(context) { /* ... same ... */ }
        function openEditEmployeeModal(rowElement, context) { /* ... same ... */ }
        function handleEmployeeFormSubmit() { /* ... same, with refined photo handling ... */ }

        // --- Other "Add" Functions (Simulated) ---
        function handleAddDocumentLink() { /* ... same ... */ }
        function handleAddImageToGallery() { /* ... same ... */ }
        function handleAdminUploadImage() { /* ... same ... */ }
        function handleCreateNotification() { /* ... same ... */ }

        // --- Create Portal User Logic (NEW) ---
        function handleCreateUser() {
            const username = document.getElementById('newUserName').value;
            const fullName = document.getElementById('newUserFullName').value;
            const email = document.getElementById('newUserEmail').value;
            const role = document.getElementById('newUserRole').value;
            const password = document.getElementById('newUserPassword').value;
            const confirmPassword = document.getElementById('newUserConfirmPassword').value;

            if (!username || !fullName || !email || !role || !password || !confirmPassword) {
                alert('Please fill all fields for the new user.');
                return;
            }
            if (password !== confirmPassword) {
                alert('Passwords do not match. Please re-enter.');
                document.getElementById('newUserConfirmPassword').focus();
                return;
            }
            if (password.length < 8) { // Basic password length check
                alert('Password should be at least 8 characters long.');
                document.getElementById('newUserPassword').focus();
                return;
            }

            // Simulate adding to the Portal Users table in Admin Panel
            const usersTableBody = document.getElementById('portalUsersTableBody');
            if (usersTableBody) {
                const newRow = usersTableBody.insertRow(0); // Add to top
                newRow.innerHTML = `
                    <td>${username}</td>
                    <td>${fullName}</td>
                    <td>${email}</td>
                    <td>${role}</td>
                    <td class="actions-cell"><button class="action-button secondary" onclick="alert('Edit User (Simulated)')">Edit</button></td>
                `;
            }

            alert(`Portal user "${username}" created successfully with role "${role}" (Simulated).`);
            closeModal('createUserModal');
        }


        // --- Initial Setup ---
        document.addEventListener('DOMContentLoaded', () => {
            showSection('dashboard', document.querySelector('.app-sidebar .nav-link'));
            window.onclick = function(event) {
                document.querySelectorAll('.modal').forEach(modal => {
                    if (event.target == modal) {
                        closeModal(modal.id);
                    }
                });
            }
            // Ensure all script contents from previous (showSection, openModal, closeModal, previewImage, employee functions, other add functions) are here.
            // For brevity, I'm assuming they are present and correctly implemented as per last version.
        });
    </script>
    <!-- Ensure all previous JavaScript is here, then add the new handleCreateUser function -->
    <script>
        // --- Utility and Core Functions --- (Copied from previous for completeness if running standalone)
        function showSection(sectionId, navLink) {
            document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.app-sidebar .nav-link').forEach(l => l.classList.remove('active'));
            const targetSection = document.getElementById(sectionId);
            if (targetSection) targetSection.classList.add('active');
            if (navLink) navLink.classList.add('active');
        }
        function openModal(modalId) { const modal = document.getElementById(modalId); if (modal) modal.style.display = 'flex'; }
        function closeModal(modalId) {
            const modal = document.getElementById(modalId); if (!modal) return;
            modal.style.display = 'none'; const form = modal.querySelector('form');
            if (form) { form.reset(); const previews = form.querySelectorAll('.image-preview'); previews.forEach(p => { p.style.display = 'none'; p.src = '#'; }); }
            if (modalId === 'employeeModal') {
                document.getElementById('employeeModalMode').value = 'add'; document.getElementById('employeeModalRowId').value = '';
                document.getElementById('employeeModalSubmitButton').textContent = 'Add Employee';
                document.getElementById('empBranchFieldContainer').style.display = 'none';
                const empIdField = document.getElementById('empId'); if (empIdField) empIdField.readOnly = false;
            }
        }
        function previewImage(event, previewElementId) {
            const reader = new FileReader(); const preview = document.getElementById(previewElementId); if (!preview) return;
            reader.onload = function(){ preview.src = reader.result; preview.style.display = 'block'; }
            if (event.target.files && event.target.files[0]) { reader.readAsDataURL(event.target.files[0]); }
            else { preview.src = '#'; preview.style.display = 'none'; }
        }
        // --- Employee Add/Edit Logic ---
        function openAddEmployeeModal(context) {
            const modal = document.getElementById('employeeModal'); document.getElementById('employeeModalTitle').textContent = `Add New Employee to ${context}`;
            document.getElementById('employeeModalMode').value = 'add'; document.getElementById('employeeModalContext').value = context;
            document.getElementById('employeeModalSubmitButton').textContent = 'Add Employee'; const empIdField = document.getElementById('empId'); if(empIdField) empIdField.readOnly = false; 
            document.getElementById('empBranchFieldContainer').style.display = (context === 'Central Directory') ? 'block' : 'none';
            modal.querySelector('form').reset(); const empPhotoPreview = document.getElementById('empPhotoPreview'); if(empPhotoPreview){empPhotoPreview.style.display = 'none'; empPhotoPreview.src = '#';}
            openModal('employeeModal');
        }
        function openEditEmployeeModal(rowElement, context) {
            const modal = document.getElementById('employeeModal'); const cells = rowElement.cells;
            document.getElementById('employeeModalTitle').textContent = `Edit Employee Details`; document.getElementById('employeeModalMode').value = 'edit';
            document.getElementById('employeeModalRowId').value = rowElement.dataset.id; document.getElementById('employeeModalContext').value = context;
            document.getElementById('employeeModalSubmitButton').textContent = 'Save Changes';
            const empIdField = document.getElementById('empId'); if(empIdField){empIdField.value = cells[1].textContent; empIdField.readOnly = true;}
            document.getElementById('empName').value = cells[2].textContent; document.getElementById('empDesignation').value = cells[3].textContent;
            const empPhotoPreview = document.getElementById('empPhotoPreview'); const storedPhotoSrc = rowElement.dataset.photoSrc; 
            if (storedPhotoSrc && storedPhotoSrc !== '#') { empPhotoPreview.src = storedPhotoSrc; empPhotoPreview.style.display = 'block'; } 
            else { const photoCellImg = cells[0].querySelector('img'); if (photoCellImg && photoCellImg.src && photoCellImg.src !== '#' && !photoCellImg.src.endsWith('/#')) { empPhotoPreview.src = photoCellImg.src; empPhotoPreview.style.display = 'block'; } else { empPhotoPreview.style.display = 'none'; empPhotoPreview.src = '#'; }}
            document.getElementById('empPhotoFile').value = '';
            if (context === 'Central Directory') { document.getElementById('empBranchFieldContainer').style.display = 'block'; document.getElementById('empBranch').value = cells[4].textContent; document.getElementById('empContact').value = cells[5].textContent; } 
            else { document.getElementById('empBranchFieldContainer').style.display = 'none'; document.getElementById('empBranch').value = context; document.getElementById('empContact').value = cells[4].textContent; }
            openModal('employeeModal');
        }
        function handleEmployeeFormSubmit() {
            const mode = document.getElementById('employeeModalMode').value; const name = document.getElementById('empName').value;
            const id = document.getElementById('empId').value; const designation = document.getElementById('empDesignation').value;
            const contact = document.getElementById('empContact').value; const context = document.getElementById('employeeModalContext').value;
            const branchForCentral = document.getElementById('empBranch').value; const photoFile = document.getElementById('empPhotoFile').files[0];
            const photoPreview = document.getElementById('empPhotoPreview'); const photoPreviewSrc = (photoPreview.style.display !== 'none' && photoPreview.src !== '#' && !photoPreview.src.endsWith('/#')) ? photoPreview.src : null;
            if (!name || !id || !designation || !contact) { alert('Please fill all required fields.'); return; }
            if (context === 'Central Directory' && !branchForCentral) { alert('Please specify the branch for Central Directory entries.'); return; }
            let photoCellHTML = `<div class="profile-placeholder-svg"><svg><use xlink:href="#profile-avatar"/></svg></div>`; let finalPhotoSrcForDataset = '';
            if (photoFile && photoPreviewSrc) { photoCellHTML = `<img src="${photoPreviewSrc}" alt="Profile">`; finalPhotoSrcForDataset = photoPreviewSrc; } 
            else if (mode === 'edit' && photoPreviewSrc) { photoCellHTML = `<img src="${photoPreviewSrc}" alt="Profile">`; finalPhotoSrcForDataset = photoPreviewSrc; }
            if (mode === 'add') {
                let targetTableBodyId; if (context === 'Data') targetTableBodyId = 'dataBranchTableBody'; else if (context === 'CC') targetTableBodyId = 'ccBranchTableBody'; else if (context === 'Central Directory') targetTableBodyId = 'centralDirectoryTableBody';
                if (targetTableBodyId) {
                    const tableBody = document.getElementById(targetTableBodyId); const newRow = tableBody.insertRow(0); newRow.dataset.id = id.replace(/\s+/g, '_') + '_' + Date.now(); newRow.dataset.photoSrc = finalPhotoSrcForDataset;
                    if (context === 'Central Directory') { newRow.innerHTML = `<td class="profile-img-cell">${photoCellHTML}</td><td>${id}</td><td>${name}</td><td>${designation}</td><td>${branchForCentral}</td><td>${contact}</td><td class="actions-cell"><button class="action-button secondary" onclick="openEditEmployeeModal(this.closest('tr'), 'Central Directory')">Edit</button></td>`; } 
                    else { newRow.innerHTML = `<td class="profile-img-cell">${photoCellHTML}</td><td>${id}</td><td>${name}</td><td>${designation}</td><td>${contact}</td><td class="actions-cell"><button class="action-button secondary" onclick="openEditEmployeeModal(this.closest('tr'), '${context}')">Edit</button></td>`; }
                } alert(`Employee "${name}" added to ${context} (Simulated).`);
            } else if (mode === 'edit') {
                const rowId = document.getElementById('employeeModalRowId').value; const rowToEdit = document.querySelector(`tr[data-id="${rowId}"]`);
                if (rowToEdit) {
                    const cells = rowToEdit.cells; cells[0].innerHTML = photoCellHTML; rowToEdit.dataset.photoSrc = finalPhotoSrcForDataset;
                    cells[2].textContent = name; cells[3].textContent = designation;
                    if (context === 'Central Directory') { cells[4].textContent = branchForCentral; cells[5].textContent = contact; } else { cells[4].textContent = contact; }
                } alert(`Employee ID "${id}" details updated (Simulated).`);
            } closeModal('employeeModal');
        }
        // --- Other "Add" Functions (Simulated) ---
        function handleAddDocumentLink() { const title = document.getElementById('docLinkTitle').value; const url = document.getElementById('docLinkUrl').value; if (!title || !url) { alert('Please enter both title and URL.'); return; } const docList = document.getElementById('documentList'); const listItem = document.createElement('li'); listItem.className = 'list-item'; listItem.innerHTML = `<span>${title}</span><a href="${url}" download class="action-button secondary">Download</a>`; docList.appendChild(listItem); alert('Document link added (Simulated).'); closeModal('addDocumentLinkModal'); }
        function handleAddImageToGallery() { const caption = document.getElementById('galleryImageCaptionPublic').value; const imageFile = document.getElementById('galleryImageFilePublic').files[0]; const imagePreview = document.getElementById('galleryImagePreviewPublic'); const imagePreviewSrc = (imagePreview.style.display !== 'none' && imagePreview.src !== '#') ? imagePreview.src : null; if (!caption || !imageFile || !imagePreviewSrc) { alert('Please select an image file and enter a caption.'); return; } const galleryContainer = document.getElementById('imageGalleryContainer'); const imgElement = document.createElement('img'); imgElement.src = imagePreviewSrc; imgElement.alt = caption; galleryContainer.appendChild(imgElement); alert('Image added to gallery (Simulated).'); closeModal('addImageToGalleryModal'); }
        function handleAdminUploadImage() { const caption = document.getElementById('adminImageCaption').value; const imageFile = document.getElementById('adminImageFileGallery').files[0]; const adminImagePreview = document.getElementById('adminImagePreviewGallery'); const imagePreviewSrc = (adminImagePreview.style.display !== 'none' && adminImagePreview.src !== '#') ? adminImagePreview.src : null; if (!caption || !imageFile || !imagePreviewSrc) { alert('Please select an image file and enter a caption for admin upload.'); return; } const galleryContainer = document.getElementById('imageGalleryContainer'); const imgElement = document.createElement('img'); imgElement.src = imagePreviewSrc; imgElement.alt = caption + " (Admin Uploaded)"; galleryContainer.appendChild(imgElement); alert('Image Upload Submitted via Admin Panel (Simulated). File: ' + imageFile.name); closeModal('adminUploadImageModal'); }
        function handleCreateNotification() { const text = document.getElementById('notificationText').value; const type = document.getElementById('notificationType').value; if (!text) { alert('Please enter notification text.'); return; } const notificationList = document.getElementById('notificationListUL'); const listItem = document.createElement('li'); listItem.className = 'list-item'; let style = ''; if (type === 'urgent') style = 'background: #fff1f0; border-left: 5px solid #ff4d4f;'; else if (type === 'alert') style = 'background: #fffbe6; border-left: 5px solid #faad14;'; else style = 'background: #e6f7ff; border-left: 5px solid #1890ff;'; listItem.style.cssText = style; listItem.innerHTML = `<strong>${type.toUpperCase()}:</strong> ${text} <small> (Just now)</small>`; notificationList.prepend(listItem); alert('Notification created (Simulated).'); closeModal('createNotificationModal'); }
        
        // --- Create Portal User Logic (NEW) ---
        function handleCreateUser() {
            const username = document.getElementById('newUserName').value;
            const fullName = document.getElementById('newUserFullName').value;
            const email = document.getElementById('newUserEmail').value;
            const role = document.getElementById('newUserRole').value;
            const password = document.getElementById('newUserPassword').value;
            const confirmPassword = document.getElementById('newUserConfirmPassword').value;

            if (!username || !fullName || !email || !role || !password || !confirmPassword) {
                alert('Please fill all fields for the new user.'); return;
            }
            if (password !== confirmPassword) {
                alert('Passwords do not match. Please re-enter.'); document.getElementById('newUserConfirmPassword').focus(); return;
            }
            if (password.length < 8) { alert('Password should be at least 8 characters long.'); document.getElementById('newUserPassword').focus(); return; }

            const usersTableBody = document.getElementById('portalUsersTableBody');
            if (usersTableBody) {
                const newRow = usersTableBody.insertRow(0);
                newRow.innerHTML = `
                    <td>${username}</td><td>${fullName}</td><td>${email}</td><td>${role}</td>
                    <td class="actions-cell"><button class="action-button secondary" onclick="alert('Edit User (Simulated)')">Edit</button></td>`;
            }
            alert(`Portal user "${username}" created successfully with role "${role}" (Simulated).`);
            closeModal('createUserModal');
        }

        // --- Initial Setup ---
        document.addEventListener('DOMContentLoaded', () => {
            showSection('dashboard', document.querySelector('.app-sidebar .nav-link'));
            window.onclick = function(event) {
                document.querySelectorAll('.modal').forEach(modal => {
                    if (event.target == modal) { closeModal(modal.id); }
                });
            }
        });
    </script>

</body>
</html>
