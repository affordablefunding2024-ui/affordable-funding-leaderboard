 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Affordable Funding - Broker Leaderboard v3</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            min-height: 100vh;
        }
        
        .trophy-gold {
            color: #FFD700;
        }
        
        .trophy-silver {
            color: #C0C0C0;
        }
        
        .trophy-bronze {
            color: #CD7F32;
        }
        
        .leaderboard-row {
            transition: all 0.3s ease;
        }
        
        .leaderboard-row:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }
        
        .sort-btn.active {
            background-color: #1e40af;
            color: white;
        }
        
        .filter-btn.active {
            background-color: #1e40af;
            color: white;
        }
        
        .modal {
            transition: opacity 0.3s ease;
        }
        
        .modal-content {
            transition: transform 0.3s ease;
        }
        
        .modal.active {
            opacity: 1;
            pointer-events: auto;
        }
        
        .modal.active .modal-content {
            transform: translateY(0);
        }
        
        .progress-bar {
            height: 8px;
            border-radius: 4px;
            background-color: #e5e7eb;
            overflow: hidden;
        }
        
        .progress-bar-fill {
            height: 100%;
            border-radius: 4px;
            transition: width 0.5s ease;
        }
        
        .circular-progress {
            position: relative;
            width: 150px;
            height: 150px;
        }
        
        .circular-progress svg {
            transform: rotate(-90deg);
        }
        
        .circular-progress circle {
            fill: none;
            stroke-width: 10;
            stroke-linecap: round;
        }
        
        .circular-progress .bg {
            stroke: #e5e7eb;
        }
        
        .circular-progress .progress {
            stroke: #1e40af;
            transition: stroke-dashoffset 0.5s ease;
        }
        
        .circular-progress .text {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
        }
        
        .team-stats-card {
            background: white;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .team-stats-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        
        #tv-mode-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 100;
        }
        
        .fullscreen-indicator {
            display: none;
            position: fixed;
            top: 10px;
            right: 10px;
            background-color: rgba(0,0,0,0.5);
            color: white;
            padding: 5px 10px;
            border-radius: 4px;
            z-index: 1000;
        }
        
        .fullscreen .fullscreen-indicator {
            display: block;
        }
        
        .broker-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            background-color: #e5e7eb;
        }
        
        .photo-preview {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            object-fit: cover;
            background-color: #e5e7eb;
            margin: 0 auto;
            display: block;
        }
        
        .photo-placeholder {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: #9ca3af;
        }
        
        .version-badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: #1e40af;
            color: white;
            padding: 2px 8px;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }
        
        .file-upload-container {
            position: relative;
            overflow: hidden;
            display: inline-block;
        }
        
        .file-upload-container input[type=file] {
            position: absolute;
            left: 0;
            top: 0;
            opacity: 0;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }
        
        .file-upload-btn {
            display: inline-block;
            padding: 8px 16px;
            background-color: #e5e7eb;
            color: #4b5563;
            border-radius: 0.375rem;
            font-size: 0.875rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .file-upload-btn:hover {
            background-color: #d1d5db;
        }
        
        .file-name {
            margin-top: 0.5rem;
            font-size: 0.75rem;
            color: #6b7280;
            text-align: center;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 100%;
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="fullscreen-indicator">
        Press ESC to exit TV mode
    </div>
    
    <div class="max-w-6xl mx-auto bg-white rounded-xl shadow-xl overflow-hidden relative">
        <div class="version-badge">v3.0</div>
        <div class="p-6 bg-gradient-to-r from-blue-800 to-blue-600 text-white">
            <div class="flex flex-col md:flex-row justify-between items-center">
                <div>
                    <h1 class="text-3xl font-bold">Affordable Funding</h1>
                    <p class="opacity-90 mt-1">Broker Performance Leaderboard</p>
                </div>
                <div class="mt-4 md:mt-0">
                    <span class="bg-blue-900 bg-opacity-50 px-4 py-2 rounded-lg">
                        Last updated: <span id="update-time">Loading...</span>
                    </span>
                </div>
            </div>
        </div>
        
        <!-- Team Progress Dashboard -->
        <div class="p-6 bg-gray-50 border-b border-gray-200">
            <h2 class="text-xl font-bold text-gray-800 mb-4">Team Progress Dashboard</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Settled Loans Progress -->
                <div class="team-stats-card p-5">
                    <h3 class="text-lg font-semibold text-gray-700 mb-3">Settled Loans Progress</h3>
                    <div class="flex items-center justify-center">
                        <div class="circular-progress" id="loans-progress-circle">
                            <svg width="150" height="150" viewBox="0 0 150 150">
                                <circle class="bg" cx="75" cy="75" r="65" />
                                <circle class="progress" cx="75" cy="75" r="65" stroke-dasharray="408" stroke-dashoffset="0" />
                            </svg>
                            <div class="text">
                                <div class="text-3xl font-bold text-blue-800" id="loans-percentage">0%</div>
                                <div class="text-sm text-gray-500" id="loans-fraction">0/0</div>
                            </div>
                        </div>
                    </div>
                    <div class="mt-4 text-center">
                        <div class="text-sm font-medium text-gray-500">Team Target: <span id="team-target-loans" class="font-bold text-gray-700">0</span> Loans</div>
                        <div class="text-sm font-medium text-gray-500">Remaining: <span id="remaining-loans" class="font-bold text-gray-700">0</span> Loans</div>
                    </div>
                </div>
                
                <!-- Revenue Progress -->
                <div class="team-stats-card p-5">
                    <h3 class="text-lg font-semibold text-gray-700 mb-3">Revenue Progress</h3>
                    <div class="mt-2">
                        <div class="flex justify-between mb-1">
                            <span class="text-sm font-medium text-gray-700" id="current-revenue">$0</span>
                            <span class="text-sm font-medium text-gray-500" id="target-revenue">$0</span>
                        </div>
                        <div class="h-4 relative w-full bg-gray-200 rounded-full overflow-hidden">
                            <div class="h-full bg-blue-600 rounded-full" id="revenue-progress-bar" style="width: 0%"></div>
                        </div>
                        <div class="mt-2 text-center">
                            <span class="text-sm font-medium text-gray-700" id="revenue-percentage">0% Complete</span>
                        </div>
                        <div class="mt-4 text-center">
                            <div class="text-sm font-medium text-gray-500">Remaining: <span id="remaining-revenue" class="font-bold text-gray-700">$0</span></div>
                        </div>
                    </div>
                </div>
                
                <!-- Team Performance Summary -->
                <div class="team-stats-card p-5">
                    <h3 class="text-lg font-semibold text-gray-700 mb-3">Team Performance</h3>
                    <div class="space-y-4">
                        <div>
                            <div class="flex justify-between">
                                <span class="text-sm font-medium text-gray-500">Total Brokers:</span>
                                <span class="text-sm font-bold text-gray-700" id="total-brokers">0</span>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between">
                                <span class="text-sm font-medium text-gray-500">Avg. Loans per Broker:</span>
                                <span class="text-sm font-bold text-gray-700" id="avg-loans">0</span>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between">
                                <span class="text-sm font-medium text-gray-500">Avg. Revenue per Broker:</span>
                                <span class="text-sm font-bold text-gray-700" id="avg-revenue">$0</span>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between">
                                <span class="text-sm font-medium text-gray-500">Top Performer:</span>
                                <span class="text-sm font-bold text-gray-700" id="top-performer">-</span>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between">
                                <span class="text-sm font-medium text-gray-500">Most Improved:</span>
                                <span class="text-sm font-bold text-blue-700" id="most-improved">-</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="p-6">
            <!-- Add Broker Button -->
            <div class="mb-6">
                <button id="add-broker-btn" class="bg-blue-700 hover:bg-blue-800 text-white font-medium py-2 px-4 rounded-lg flex items-center">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z" clip-rule="evenodd" />
                    </svg>
                    Add Broker
                </button>
                
                <!-- Data Management Buttons -->
                <div class="inline-flex ml-4">
                    <button id="save-data-btn" class="bg-green-600 hover:bg-green-700 text-white font-medium py-2 px-4 rounded-l-lg">
                        Save Data
                    </button>
                    <button id="reset-data-btn" class="bg-red-600 hover:bg-red-700 text-white font-medium py-2 px-4 rounded-r-lg">
                        Reset Data
                    </button>
                </div>
            </div>
            
            <!-- Search and Filter Controls -->
            <div class="flex flex-col md:flex-row justify-between items-center mb-6 gap-4">
                <div class="w-full md:w-auto">
                    <input type="text" id="search" placeholder="Search brokers..." 
                           class="px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 w-full">
                </div>
                
                <div class="flex flex-wrap gap-2 justify-center md:justify-end w-full md:w-auto">
                    <button class="sort-btn px-3 py-1 rounded-md border border-gray-300 hover:bg-gray-100 active" data-sort="settledLoans">
                        Settled Loans
                    </button>
                    <button class="sort-btn px-3 py-1 rounded-md border border-gray-300 hover:bg-gray-100" data-sort="name">
                        Name
                    </button>
                    <button class="sort-btn px-3 py-1 rounded-md border border-gray-300 hover:bg-gray-100" data-sort="revenue">
                        Revenue
                    </button>
                </div>
            </div>
            
            <div class="mb-6 flex flex-wrap gap-2">
                <span class="font-medium text-gray-700 mr-2">Filter by:</span>
                <button class="filter-btn px-3 py-1 rounded-md border border-gray-300 hover:bg-gray-100 active" data-filter="all">
                    All Brokers
                </button>
                <button class="filter-btn px-3 py-1 rounded-md border border-gray-300 hover:bg-gray-100" data-filter="top">
                    Top 5
                </button>
                <button class="filter-btn px-3 py-1 rounded-md border border-gray-300 hover:bg-gray-100" data-filter="target-close">
                    Close to Target
                </button>
            </div>
            
            <!-- Leaderboard Table -->
            <div class="overflow-x-auto">
                <table class="min-w-full">
                    <thead>
                        <tr class="bg-gray-100">
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Rank</th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Broker</th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Settled Loans</th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Target Loans</th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Revenue</th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Target Revenue</th>
                            <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Progress</th>
                            <th class="px-4 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="leaderboard-body">
                        <!-- Leaderboard data will be inserted here -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    
    <!-- TV Mode Button -->
    <button id="tv-mode-btn" class="bg-blue-700 hover:bg-blue-800 text-white font-medium py-2 px-4 rounded-full shadow-lg flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M3 5a2 2 0 012-2h10a2 2 0 012 2v8a2 2 0 01-2 2h-2.22l.123.489.804.804A1 1 0 0113 18H7a1 1 0 01-.707-1.707l.804-.804L7.22 15H5a2 2 0 01-2-2V5zm5.771 7H5V5h10v7H8.771z" clip-rule="evenodd" />
        </svg>
        TV Mode
    </button>
    
    <!-- Add/Edit Broker Modal -->
    <div id="broker-modal" class="modal fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 opacity-0 pointer-events-none">
        <div class="modal-content bg-white rounded-lg shadow-xl max-w-md w-full mx-4 transform translate-y-8">
            <div class="p-6">
                <h3 id="modal-title" class="text-lg font-bold text-gray-900 mb-4">Add New Broker</h3>
                
                <form id="broker-form">
                    <input type="hidden" id="broker-id">
                    
                    <!-- Photo Preview -->
                    <div class="mb-6">
                        <div class="photo-preview" id="photo-preview">
                            <div class="photo-placeholder">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                                </svg>
                            </div>
                        </div>
                    </div>
                    
                    <div class="mb-4">
                        <label for="broker-name" class="block text-sm font-medium text-gray-700 mb-1">Broker Name</label>
                        <input type="text" id="broker-name" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                    </div>
                    
                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Broker Photo</label>
                        <div class="file-upload-container w-full">
                            <div class="file-upload-btn w-full text-center">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 inline-block mr-1" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M4 5a2 2 0 00-2 2v8a2 2 0 002 2h12a2 2 0 002-2V7a2 2 0 00-2-2h-1.586a1 1 0 01-.707-.293l-1.121-1.121A2 2 0 0011.172 3H8.828a2 2 0 00-1.414.586L6.293 4.707A1 1 0 015.586 5H4zm6 9a3 3 0 100-6 3 3 0 000 6z" clip-rule="evenodd" />
                                </svg>
                                Choose Photo
                            </div>
                            <input type="file" id="photo-upload" accept="image/*">
                        </div>
                        <div class="file-name" id="file-name">No file chosen</div>
                        <input type="hidden" id="photo-data">
                    </div>
                    
                    <div class="mb-4">
                        <label for="settled-loans" class="block text-sm font-medium text-gray-700 mb-1">Settled Loans</label>
                        <input type="number" id="settled-loans" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" min="0" required>
                    </div>
                    
                    <div class="mb-4">
                        <label for="target-loans" class="block text-sm font-medium text-gray-700 mb-1">Target Number of Settled Loans</label>
                        <input type="number" id="target-loans" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" min="0" required>
                    </div>
                    
                    <div class="mb-4">
                        <label for="revenue" class="block text-sm font-medium text-gray-700 mb-1">Revenue ($)</label>
                        <input type="number" id="revenue" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" min="0" required>
                    </div>
                    
                    <div class="mb-4">
                        <label for="target-revenue" class="block text-sm font-medium text-gray-700 mb-1">Target Revenue ($)</label>
                        <input type="number" id="target-revenue" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" min="0" required>
                    </div>
                    
                    <div class="flex justify-end gap-3 mt-6">
                        <button type="button" id="cancel-btn" class="px-4 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50">
                            Cancel
                        </button>
                        <button type="submit" id="save-btn" class="px-4 py-2 bg-blue-700 text-white rounded-md hover:bg-blue-800">
                            Save Broker
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    
    <!-- Confirmation Modal -->
    <div id="confirm-modal" class="modal fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 opacity-0 pointer-events-none">
        <div class="modal-content bg-white rounded-lg shadow-xl max-w-md w-full mx-4 transform translate-y-8">
            <div class="p-6">
                <h3 class="text-lg font-bold text-gray-900 mb-2">Confirm Deletion</h3>
                <p class="text-gray-600 mb-6">Are you sure you want to delete this broker? This action cannot be undone.</p>
                
                <div class="flex justify-end gap-3">
                    <button id="cancel-delete-btn" class="px-4 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50">
                        Cancel
                    </button>
                    <button id="confirm-delete-btn" class="px-4 py-2 bg-red-600 text-white rounded-md hover:bg-red-700">
                        Delete
                    </button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Reset Confirmation Modal -->
    <div id="reset-confirm-modal" class="modal fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 opacity-0 pointer-events-none">
        <div class="modal-content bg-white rounded-lg shadow-xl max-w-md w-full mx-4 transform translate-y-8">
            <div class="p-6">
                <h3 class="text-lg font-bold text-gray-900 mb-2">Reset Data</h3>
                <p class="text-gray-600 mb-6">Are you sure you want to reset all data to default? This action cannot be undone.</p>
                
                <div class="flex justify-end gap-3">
                    <button id="cancel-reset-btn" class="px-4 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50">
                        Cancel
                    </button>
                    <button id="confirm-reset-btn" class="px-4 py-2 bg-red-600 text-white rounded-md hover:bg-red-700">
                        Reset Data
                    </button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Photo Preview Modal -->
    <div id="photo-modal" class="modal fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center z-50 opacity-0 pointer-events-none">
        <div class="modal-content bg-white rounded-lg shadow-xl max-w-lg w-full mx-4 transform translate-y-8">
            <div class="p-6">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-lg font-bold text-gray-900" id="photo-modal-name">Broker Name</h3>
                    <button id="close-photo-modal" class="text-gray-500 hover:text-gray-700">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
                
                <div class="flex justify-center">
                    <img id="photo-modal-image" class="max-h-96 rounded-lg" src="" alt="Broker Photo">
                </div>
                
                <div class="mt-6 text-center">
                    <div class="text-sm font-medium text-gray-700" id="photo-modal-stats"></div>
                </div>
            </div>
        </div>
    </div>
    
    <script>
        // Default broker data with photo URLs
        const defaultBrokerData = [
            { 
                id: 1, 
                rank: 1, 
                name: "Michael Johnson", 
                settledLoans: 42, 
                targetLoans: 50, 
                revenue: 210000, 
                targetRevenue: 250000,
                previousLoans: 38,
                photoUrl: "https://randomuser.me/api/portraits/men/32.jpg"
            },
            { 
                id: 2, 
                rank: 2, 
                name: "Sarah Williams", 
                settledLoans: 38, 
                targetLoans: 45, 
                revenue: 190000, 
                targetRevenue: 225000,
                previousLoans: 32,
                photoUrl: "https://randomuser.me/api/portraits/women/44.jpg"
            },
            { 
                id: 3, 
                rank: 3, 
                name: "David Chen", 
                settledLoans: 35, 
                targetLoans: 45, 
                revenue: 175000, 
                targetRevenue: 225000,
                previousLoans: 30,
                photoUrl: "https://randomuser.me/api/portraits/men/67.jpg"
            },
            { 
                id: 4, 
                rank: 4, 
                name: "Jessica Taylor", 
                settledLoans: 32, 
                targetLoans: 40, 
                revenue: 160000, 
                targetRevenue: 200000,
                previousLoans: 29,
                photoUrl: "https://randomuser.me/api/portraits/women/17.jpg"
            },
            { 
                id: 5, 
                rank: 5, 
                name: "Robert Garcia", 
                settledLoans: 30, 
                targetLoans: 40, 
                revenue: 150000, 
                targetRevenue: 200000,
                previousLoans: 25,
                photoUrl: "https://randomuser.me/api/portraits/men/91.jpg"
            },
            { 
                id: 6, 
                rank: 6, 
                name: "Emily Wilson", 
                settledLoans: 28, 
                targetLoans: 35, 
                revenue: 140000, 
                targetRevenue: 175000,
                previousLoans: 25,
                photoUrl: "https://randomuser.me/api/portraits/women/28.jpg"
            },
            { 
                id: 7, 
                rank: 7, 
                name: "James Brown", 
                settledLoans: 25, 
                targetLoans: 35, 
                revenue: 125000, 
                targetRevenue: 175000,
                previousLoans: 22,
                photoUrl: "https://randomuser.me/api/portraits/men/55.jpg"
            },
            { 
                id: 8, 
                rank: 8, 
                name: "Amanda Lee", 
                settledLoans: 22, 
                targetLoans: 30, 
                revenue: 110000, 
                targetRevenue: 150000,
                previousLoans: 20,
                photoUrl: "https://randomuser.me/api/portraits/women/63.jpg"
            },
            { 
                id: 9, 
                rank: 9, 
                name: "Thomas Martinez", 
                settledLoans: 18, 
                targetLoans: 30, 
                revenue: 90000, 
                targetRevenue: 150000,
                previousLoans: 15,
                photoUrl: "https://randomuser.me/api/portraits/men/41.jpg"
            }
        ];
        
        // Load broker data from localStorage or use default
        let brokerData = JSON.parse(localStorage.getItem('brokerData')) || [...defaultBrokerData];
        
        // Current sort and filter states
        let currentSort = { field: 'settledLoans', ascending: false };
        let currentFilter = 'all';
        let searchTerm = '';
        let brokerToDelete = null;
        let isInTVMode = false;
        
        // Function to save data to localStorage
        function saveDataToLocalStorage() {
            localStorage.setItem('brokerData', JSON.stringify(brokerData));
            
            // Show save confirmation
            const saveBtn = document.getElementById('save-data-btn');
            const originalText = saveBtn.textContent;
            saveBtn.textContent = 'Saved!';
            saveBtn.classList.add('bg-green-500');
            
            setTimeout(() => {
                saveBtn.textContent = originalText;
                saveBtn.classList.remove('bg-green-500');
            }, 2000);
        }
        
        // Function to reset data to default
        function resetDataToDefault() {
            brokerData = [...defaultBrokerData];
            saveDataToLocalStorage();
            renderLeaderboard();
            
            // Close modal
            document.getElementById('reset-confirm-modal').classList.remove('active');
        }
        
        // Function to toggle TV mode
        function toggleTVMode() {
            isInTVMode = !isInTVMode;
            
            if (isInTVMode) {
                // Enter TV mode
                document.documentElement.requestFullscreen().catch(err => {
                    console.log(`Error attempting to enable fullscreen: ${err.message}`);
                });
                document.body.classList.add('fullscreen');
                document.getElementById('tv-mode-btn').style.display = 'none';
                
                // Hide edit controls in TV mode
                document.querySelectorAll('.edit-btn, .delete-btn, #add-broker-btn, #save-data-btn, #reset-data-btn').forEach(el => {
                    el.style.display = 'none';
                });
                
                // Auto-refresh every 5 minutes
                window.tvModeInterval = setInterval(() => {
                    renderLeaderboard();
                }, 300000); // 5 minutes
            } else {
                // Exit TV mode
                if (document.fullscreenElement) {
                    document.exitFullscreen();
                }
                document.body.classList.remove('fullscreen');
                document.getElementById('tv-mode-btn').style.display = 'flex';
                
                // Show edit controls again
                document.querySelectorAll('.edit-btn, .delete-btn, #add-broker-btn, #save-data-btn, #reset-data-btn').forEach(el => {
                    el.style.display = '';
                });
                
                // Clear auto-refresh
                clearInterval(window.tvModeInterval);
            }
        }
        
        // Listen for fullscreen change
        document.addEventListener('fullscreenchange', () => {
            if (!document.fullscreenElement && isInTVMode) {
                toggleTVMode();
            }
        });
        
        // Function to calculate team totals
        function calculateTeamTotals() {
            const totals = {
                totalBrokers: brokerData.length,
                totalSettledLoans: 0,
                totalTargetLoans: 0,
                totalRevenue: 0,
                totalTargetRevenue: 0,
                topPerformer: null,
                mostImproved: null
            };
            
            let maxLoans = 0;
            let maxImprovement = 0;
            
            brokerData.forEach(broker => {
                totals.totalSettledLoans += broker.settledLoans;
                totals.totalTargetLoans += broker.targetLoans;
                totals.totalRevenue += broker.revenue;
                totals.totalTargetRevenue += broker.targetRevenue;
                
                // Find top performer
                if (broker.settledLoans > maxLoans) {
                    maxLoans = broker.settledLoans;
                    totals.topPerformer = broker.name;
                }
                
                // Find most improved
                const improvement = broker.settledLoans - broker.previousLoans;
                if (improvement > maxImprovement) {
                    maxImprovement = improvement;
                    totals.mostImproved = broker.name;
                }
            });
            
            totals.avgLoans = totals.totalBrokers > 0 ? Math.round(totals.totalSettledLoans / totals.totalBrokers * 10) / 10 : 0;
            totals.avgRevenue = totals.totalBrokers > 0 ? Math.round(totals.totalRevenue / totals.totalBrokers) : 0;
            totals.remainingLoans = Math.max(0, totals.totalTargetLoans - totals.totalSettledLoans);
            totals.remainingRevenue = Math.max(0, totals.totalTargetRevenue - totals.totalRevenue);
            totals.loansPercentage = totals.totalTargetLoans > 0 ? Math.round((totals.totalSettledLoans / totals.totalTargetLoans) * 100) : 0;
            totals.revenuePercentage = totals.totalTargetRevenue > 0 ? Math.round((totals.totalRevenue / totals.totalTargetRevenue) * 100) : 0;
            
            return totals;
        }
        
        // Function to update team progress dashboard
        function updateTeamDashboard() {
            const totals = calculateTeamTotals();
            
            // Update loans progress circle
            const loansCircle = document.querySelector('#loans-progress-circle .progress');
            const circumference = 2 * Math.PI * 65; // 2πr where r=65
            const offset = circumference - (totals.loansPercentage / 100) * circumference;
            loansCircle.style.strokeDasharray = `${circumference} ${circumference}`;
            loansCircle.style.strokeDashoffset = offset;
            
            // Update loans percentage and fraction
            document.getElementById('loans-percentage').textContent = `${totals.loansPercentage}%`;
            document.getElementById('loans-fraction').textContent = `${totals.totalSettledLoans}/${totals.totalTargetLoans}`;
            document.getElementById('team-target-loans').textContent = totals.totalTargetLoans;
            document.getElementById('remaining-loans').textContent = totals.remainingLoans;
            
            // Update revenue progress bar
            document.getElementById('revenue-progress-bar').style.width = `${totals.revenuePercentage}%`;
            document.getElementById('current-revenue').textContent = `$${totals.totalRevenue.toLocaleString()}`;
            document.getElementById('target-revenue').textContent = `$${totals.totalTargetRevenue.toLocaleString()}`;
            document.getElementById('revenue-percentage').textContent = `${totals.revenuePercentage}% Complete`;
            document.getElementById('remaining-revenue').textContent = `$${totals.remainingRevenue.toLocaleString()}`;
            
            // Update team performance summary
            document.getElementById('total-brokers').textContent = totals.totalBrokers;
            document.getElementById('avg-loans').textContent = totals.avgLoans;
            document.getElementById('avg-revenue').textContent = `$${totals.avgRevenue.toLocaleString()}`;
            document.getElementById('top-performer').textContent = totals.topPerformer || '-';
            document.getElementById('most-improved').textContent = totals.mostImproved ? `${totals.mostImproved}` : '-';
            
            // Set color of progress indicators based on percentage
            if (totals.loansPercentage < 50) {
                loansCircle.style.stroke = '#ef4444'; // Red
            } else if (totals.loansPercentage < 80) {
                loansCircle.style.stroke = '#f59e0b'; // Amber
            } else {
                loansCircle.style.stroke = '#10b981'; // Green
            }
            
            if (totals.revenuePercentage < 50) {
                document.getElementById('revenue-progress-bar').className = 'h-full bg-red-500 rounded-full';
            } else if (totals.revenuePercentage < 80) {
                document.getElementById('revenue-progress-bar').className = 'h-full bg-yellow-500 rounded-full';
            } else {
                document.getElementById('revenue-progress-bar').className = 'h-full bg-green-500 rounded-full';
            }
        }
        
        // Function to calculate ranks based on settled loans
        function calculateRanks() {
            // Sort by settled loans in descending order
            const sortedBrokers = [...brokerData].sort((a, b) => b.settledLoans - a.settledLoans);
            
            // Assign ranks
            sortedBrokers.forEach((broker, index) => {
                broker.rank = index + 1;
            });
        }
        
        // Function to render the leaderboard
        function renderLeaderboard() {
            const leaderboardBody = document.getElementById('leaderboard-body');
            leaderboardBody.innerHTML = '';
            
            // Calculate ranks first
            calculateRanks();
            
            // Update team dashboard
            updateTeamDashboard();
            
            // Filter data
            let filteredData = [...brokerData];
            
            if (currentFilter === 'top') {
                filteredData = filteredData.slice(0, 5);
            } else if (currentFilter === 'target-close') {
                // Filter brokers who are at least 80% to their target
                filteredData = filteredData.filter(broker => 
                    (broker.settledLoans / broker.targetLoans) >= 0.8
                );
            }
            
            // Apply search
            if (searchTerm) {
                filteredData = filteredData.filter(broker => 
                    broker.name.toLowerCase().includes(searchTerm.toLowerCase())
                );
            }
            
            // Sort data
            filteredData.sort((a, b) => {
                let valueA = a[currentSort.field];
                let valueB = b[currentSort.field];
                
                if (typeof valueA === 'string') {
                    valueA = valueA.toLowerCase();
                    valueB = valueB.toLowerCase();
                }
                
                if (valueA < valueB) return currentSort.ascending ? -1 : 1;
                if (valueA > valueB) return currentSort.ascending ? 1 : -1;
                return 0;
            });
            
            // Render rows
            filteredData.forEach(broker => {
                const row = document.createElement('tr');
                row.className = 'leaderboard-row border-b hover:bg-gray-50';
                row.setAttribute('data-id', broker.id);
                
                // Calculate progress percentages
                const loansProgress = Math.min(100, Math.round((broker.settledLoans / broker.targetLoans) * 100));
                const revenueProgress = Math.min(100, Math.round((broker.revenue / broker.targetRevenue) * 100));
                
                // Determine progress color
                let progressColor;
                if (loansProgress < 50) {
                    progressColor = 'bg-red-500';
                } else if (loansProgress < 80) {
                    progressColor = 'bg-yellow-500';
                } else {
                    progressColor = 'bg-green-500';
                }
                
                // Determine trophy or rank number
                let rankDisplay;
                if (broker.rank === 1) {
                    rankDisplay = `<span class="trophy-gold text-xl">🏆</span>`;
                } else if (broker.rank === 2) {
                    rankDisplay = `<span class="trophy-silver text-xl">🥈</span>`;
                } else if (broker.rank === 3) {
                    rankDisplay = `<span class="trophy-bronze text-xl">🥉</span>`;
                } else {
                    rankDisplay = `<span class="font-semibold">${broker.rank}</span>`;
                }
                
                // Calculate remaining loans needed
                const remainingLoans = Math.max(0, broker.targetLoans - broker.settledLoans);
                
                // Determine broker avatar (photo or initials)
                let brokerAvatar;
                if (broker.photoUrl) {
                    brokerAvatar = `
                        <img src="${broker.photoUrl}" alt="${broker.name}" class="broker-avatar view-photo" data-id="${broker.id}">
                    `;
                } else {
                    brokerAvatar = `
                        <div class="h-10 w-10 rounded-full bg-blue-100 flex items-center justify-center text-blue-800 font-bold">
                            ${broker.name.charAt(0)}
                        </div>
                    `;
                }
                
                row.innerHTML = `
                    <td class="px-4 py-4 whitespace-nowrap">${rankDisplay}</td>
                    <td class="px-4 py-4 whitespace-nowrap">
                        <div class="flex items-center">
                            ${brokerAvatar}
                            <div class="ml-4">
                                <div class="text-sm font-medium text-gray-900">${broker.name}</div>
                            </div>
                        </div>
                    </td>
                    <td class="px-4 py-4 whitespace-nowrap">
                        <div class="text-sm font-medium text-gray-900">${broker.settledLoans}</div>
                    </td>
                    <td class="px-4 py-4 whitespace-nowrap">
                        <div class="text-sm text-gray-900">${broker.targetLoans}</div>
                        <div class="text-xs text-gray-500">${remainingLoans} more needed</div>
                    </td>
                    <td class="px-4 py-4 whitespace-nowrap">
                        <div class="text-sm font-medium text-gray-900">$${broker.revenue.toLocaleString()}</div>
                    </td>
                    <td class="px-4 py-4 whitespace-nowrap">
                        <div class="text-sm text-gray-900">$${broker.targetRevenue.toLocaleString()}</div>
                    </td>
                    <td class="px-4 py-4 whitespace-nowrap">
                        <div class="w-32">
                            <div class="text-xs font-medium text-gray-900 mb-1">${loansProgress}% Complete</div>
                            <div class="progress-bar">
                                <div class="progress-bar-fill ${progressColor}" style="width: ${loansProgress}%"></div>
                            </div>
                        </div>
                    </td>
                    <td class="px-4 py-4 whitespace-nowrap text-right text-sm font-medium">
                        <button class="edit-btn text-blue-600 hover:text-blue-900 mr-3" data-id="${broker.id}">
                            Edit
                        </button>
                        <button class="delete-btn text-red-600 hover:text-red-900" data-id="${broker.id}">
                            Delete
                        </button>
                    </td>
                `;
                
                leaderboardBody.appendChild(row);
            });
            
            // Update "no results" message if needed
            if (filteredData.length === 0) {
                const noResultsRow = document.createElement('tr');
                noResultsRow.innerHTML = `
                    <td colspan="8" class="px-6 py-4 text-center text-gray-500">
                        No brokers match your search criteria
                    </td>
                `;
                leaderboardBody.appendChild(noResultsRow);
            }
            
            // Update event listeners for edit and delete buttons
            document.querySelectorAll('.edit-btn').forEach(button => {
                button.addEventListener('click', handleEditBroker);
            });
            
            document.querySelectorAll('.delete-btn').forEach(button => {
                button.addEventListener('click', handleDeleteBroker);
            });
            
            // Add event listeners for photo viewing
            document.querySelectorAll('.view-photo').forEach(photo => {
                photo.addEventListener('click', handleViewPhoto);
            });
            
            // Update timestamp
            updateTimestamp();
        }
        
        // Function to update the timestamp
        function updateTimestamp() {
            const now = new Date();
            document.getElementById('update-time').textContent = now.toLocaleDateString('en-US', { 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
        }
        
        // Function to handle viewing a broker's photo
        function handleViewPhoto(e) {
            const brokerId = parseInt(e.currentTarget.getAttribute('data-id'));
            const broker = brokerData.find(b => b.id === brokerId);
            
            if (broker && broker.photoUrl) {
                // Set photo modal content
                document.getElementById('photo-modal-name').textContent = broker.name;
                document.getElementById('photo-modal-image').src = broker.photoUrl;
                document.getElementById('photo-modal-image').alt = broker.name;
                
                // Set broker stats
                const loansProgress = Math.round((broker.settledLoans / broker.targetLoans) * 100);
                document.getElementById('photo-modal-stats').innerHTML = `
                    Rank: ${broker.rank} | Settled Loans: ${broker.settledLoans}/${broker.targetLoans} (${loansProgress}%) | Revenue: $${broker.revenue.toLocaleString()}
                `;
                
                // Show modal
                document.getElementById('photo-modal').classList.add('active');
            }
        }
        
        // Function to close photo modal
        function closePhotoModal() {
            document.getElementById('photo-modal').classList.remove('active');
        }
        
        // Function to handle file selection for broker photo
        function handleFileSelect(e) {
            const file = e.target.files[0];
            if (!file) return;
            
            // Update file name display
            document.getElementById('file-name').textContent = file.name;
            
            // Check if file is an image
            if (!file.type.match('image.*')) {
                alert('Please select an image file.');
                return;
            }
            
            // Check file size (limit to 2MB)
            if (file.size > 2 * 1024 * 1024) {
                alert('Image size should be less than 2MB.');
                return;
            }
            
            // Read the file and convert to data URL
            const reader = new FileReader();
            reader.onload = function(e) {
                const dataUrl = e.target.result;
                
                // Update photo preview
                const photoPreview = document.getElementById('photo-preview');
                photoPreview.innerHTML = `<img src="${dataUrl}" alt="Broker Photo" class="photo-preview">`;
                
                // Store data URL in hidden input
                document.getElementById('photo-data').value = dataUrl;
            };
            reader.readAsDataURL(file);
        }
        
        // Function to open the broker modal
        function openBrokerModal(isEdit = false, brokerId = null) {
            const modal = document.getElementById('broker-modal');
            const modalTitle = document.getElementById('modal-title');
            const form = document.getElementById('broker-form');
            const idInput = document.getElementById('broker-id');
            const nameInput = document.getElementById('broker-name');
            const photoPreview = document.getElementById('photo-preview');
            const fileNameDisplay = document.getElementById('file-name');
            const photoDataInput = document.getElementById('photo-data');
            const settledLoansInput = document.getElementById('settled-loans');
            const targetLoansInput = document.getElementById('target-loans');
            const revenueInput = document.getElementById('revenue');
            const targetRevenueInput = document.getElementById('target-revenue');
            
            // Reset form
            form.reset();
            photoDataInput.value = '';
            fileNameDisplay.textContent = 'No file chosen';
            
            // Reset photo preview
            photoPreview.innerHTML = `
                <div class="photo-placeholder">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                    </svg>
                </div>
            `;
            
            if (isEdit && brokerId) {
                // Find the broker
                const broker = brokerData.find(b => b.id === brokerId);
                if (broker) {
                    modalTitle.textContent = 'Edit Broker';
                    idInput.value = broker.id;
                    nameInput.value = broker.name;
                    settledLoansInput.value = broker.settledLoans;
                    targetLoansInput.value = broker.targetLoans;
                    revenueInput.value = broker.revenue;
                    targetRevenueInput.value = broker.targetRevenue;
                    
                    // Update photo preview if available
                    if (broker.photoUrl) {
                        photoPreview.innerHTML = `<img src="${broker.photoUrl}" alt="${broker.name}" class="photo-preview">`;
                        photoDataInput.value = broker.photoUrl;
                        fileNameDisplay.textContent = 'Current photo';
                    }
                }
            } else {
                modalTitle.textContent = 'Add New Broker';
                idInput.value = '';
            }
            
            // Show modal
            modal.classList.add('active');
        }
        
        // Function to close the broker modal
        function closeBrokerModal() {
            const modal = document.getElementById('broker-modal');
            modal.classList.remove('active');
        }
        
        // Function to handle form submission
        function handleFormSubmit(e) {
            e.preventDefault();
            
            const idInput = document.getElementById('broker-id');
            const nameInput = document.getElementById('broker-name');
            const photoDataInput = document.getElementById('photo-data');
            const settledLoansInput = document.getElementById('settled-loans');
            const targetLoansInput = document.getElementById('target-loans');
            const revenueInput = document.getElementById('revenue');
            const targetRevenueInput = document.getElementById('target-revenue');
            
            const brokerId = idInput.value ? parseInt(idInput.value) : null;
            const brokerName = nameInput.value.trim();
            const photoUrl = photoDataInput.value.trim();
            const settledLoans = parseInt(settledLoansInput.value);
            const targetLoans = parseInt(targetLoansInput.value);
            const revenue = parseInt(revenueInput.value);
            const targetRevenue = parseInt(targetRevenueInput.value);
            
            if (!brokerName || isNaN(settledLoans) || isNaN(targetLoans) || isNaN(revenue) || isNaN(targetRevenue)) {
                alert('Please fill in all required fields with valid numbers.');
                return;
            }
            
            if (brokerId) {
                // Edit existing broker
                const brokerIndex = brokerData.findIndex(b => b.id === brokerId);
                if (brokerIndex !== -1) {
                    // Store previous loans for improvement tracking
                    const previousLoans = brokerData[brokerIndex].settledLoans;
                    
                    brokerData[brokerIndex] = {
                        ...brokerData[brokerIndex],
                        name: brokerName,
                        photoUrl: photoUrl || brokerData[brokerIndex].photoUrl, // Keep existing photo if no new one
                        settledLoans: settledLoans,
                        targetLoans: targetLoans,
                        revenue: revenue,
                        targetRevenue: targetRevenue,
                        previousLoans: previousLoans // Keep track of previous value
                    };
                }
            } else {
                // Add new broker
                const newId = brokerData.length > 0 ? Math.max(...brokerData.map(b => b.id)) + 1 : 1;
                brokerData.push({
                    id: newId,
                    rank: 0, // Will be calculated
                    name: brokerName,
                    photoUrl: photoUrl || null,
                    settledLoans: settledLoans,
                    targetLoans: targetLoans,
                    revenue: revenue,
                    targetRevenue: targetRevenue,
                    previousLoans: settledLoans - 5 // Assume some previous value for new brokers
                });
            }
            
            // Save to localStorage
            saveDataToLocalStorage();
            
            // Close modal and update leaderboard
            closeBrokerModal();
            renderLeaderboard();
        }
        
        // Function to handle edit broker
        function handleEditBroker(e) {
            const brokerId = parseInt(e.currentTarget.getAttribute('data-id'));
            openBrokerModal(true, brokerId);
        }
        
        // Function to handle delete broker
        function handleDeleteBroker(e) {
            const brokerId = parseInt(e.currentTarget.getAttribute('data-id'));
            brokerToDelete = brokerId;
            
            // Open confirmation modal
            document.getElementById('confirm-modal').classList.add('active');
        }
        
        // Function to confirm delete broker
        function confirmDeleteBroker() {
            if (brokerToDelete !== null) {
                brokerData = brokerData.filter(broker => broker.id !== brokerToDelete);
                brokerToDelete = null;
                
                // Save to localStorage
                saveDataToLocalStorage();
                
                // Close modal and update leaderboard
                document.getElementById('confirm-modal').classList.remove('active');
                renderLeaderboard();
            }
        }
        
        // Function to cancel delete broker
        function cancelDeleteBroker() {
            brokerToDelete = null;
            document.getElementById('confirm-modal').classList.remove('active');
        }
        
        // Initialize the leaderboard
        document.addEventListener('DOMContentLoaded', () => {
            // Set up sort buttons
            document.querySelectorAll('.sort-btn').forEach(button => {
                button.addEventListener('click', () => {
                    const sortField = button.getAttribute('data-sort');
                    
                    // Toggle sort direction if clicking the same field
                    if (currentSort.field === sortField) {
                        currentSort.ascending = !currentSort.ascending;
                    } else {
                        currentSort.field = sortField;
                        // Default to descending for numeric fields (higher is better)
                        currentSort.ascending = sortField === 'name';
                    }
                    
                    // Update active button
                    document.querySelectorAll('.sort-btn').forEach(btn => {
                        btn.classList.remove('active');
                    });
                    button.classList.add('active');
                    
                    renderLeaderboard();
                });
            });
            
            // Set up filter buttons
            document.querySelectorAll('.filter-btn').forEach(button => {
                button.addEventListener('click', () => {
                    currentFilter = button.getAttribute('data-filter');
                    
                    // Update active button
                    document.querySelectorAll('.filter-btn').forEach(btn => {
                        btn.classList.remove('active');
                    });
                    button.classList.add('active');
                    
                    renderLeaderboard();
                });
            });
            
            // Set up search
            document.getElementById('search').addEventListener('input', (e) => {
                searchTerm = e.target.value;
                renderLeaderboard();
            });
            
            // Set up add broker button
            document.getElementById('add-broker-btn').addEventListener('click', () => {
                openBrokerModal();
            });
            
            // Set up photo upload
            document.getElementById('photo-upload').addEventListener('change', handleFileSelect);
            
            // Set up form submission
            document.getElementById('broker-form').addEventListener('submit', handleFormSubmit);
            
            // Set up cancel button
            document.getElementById('cancel-btn').addEventListener('click', closeBrokerModal);
            
            // Set up delete confirmation
            document.getElementById('confirm-delete-btn').addEventListener('click', confirmDeleteBroker);
            document.getElementById('cancel-delete-btn').addEventListener('click', cancelDeleteBroker);
            
            // Set up photo modal close button
            document.getElementById('close-photo-modal').addEventListener('click', closePhotoModal);
            
            // Set up save data button
            document.getElementById('save-data-btn').addEventListener('click', saveDataToLocalStorage);
            
            // Set up reset data button
            document.getElementById('reset-data-btn').addEventListener('click', () => {
                document.getElementById('reset-confirm-modal').classList.add('active');
            });
            
            // Set up reset confirmation
            document.getElementById('confirm-reset-btn').addEventListener('click', resetDataToDefault);
            document.getElementById('cancel-reset-btn').addEventListener('click', () => {
                document.getElementById('reset-confirm-modal').classList.remove('active');
            });
            
            // Set up TV mode button
            document.getElementById('tv-mode-btn').addEventListener('click', toggleTVMode);
            
            // Initial render
            renderLeaderboard();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9681916696ac7136',t:'MTc1NDAxMTgwMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
