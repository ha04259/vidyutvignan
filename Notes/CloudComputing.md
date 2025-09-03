<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cloud Computing Models Guide</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
            font-size: 2.5em;
            background: linear-gradient(45deg, #3498db, #9b59b6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        h2 {
            color: #34495e;
            border-bottom: 3px solid #3498db;
            padding-bottom: 10px;
            margin-top: 40px;
        }
        
        .models-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        
        .model-card {
            border: 2px solid #ecf0f1;
            border-radius: 10px;
            padding: 20px;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .model-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(45deg, #3498db, #2ecc71);
        }
        
        .model-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
        }
        
        .iaas { border-color: #e74c3c; }
        .iaas::before { background: linear-gradient(45deg, #e74c3c, #c0392b); }
        
        .paas { border-color: #f39c12; }
        .paas::before { background: linear-gradient(45deg, #f39c12, #d68910); }
        
        .saas { border-color: #2ecc71; }
        .saas::before { background: linear-gradient(45deg, #2ecc71, #27ae60); }
        
        .traditional { border-color: #9b59b6; }
        .traditional::before { background: linear-gradient(45deg, #9b59b6, #8e44ad); }
        
        .model-title {
            font-size: 1.4em;
            font-weight: bold;
            margin-bottom: 15px;
        }
        
        .stack-diagram {
            display: flex;
            flex-direction: column;
            gap: 3px;
            margin: 20px 0;
        }
        
        .stack-layer {
            padding: 10px;
            text-align: center;
            border-radius: 5px;
            font-weight: 500;
            font-size: 0.9em;
            transition: all 0.3s ease;
        }
        
        .stack-layer:hover {
            transform: scale(1.02);
        }
        
        .managed { background: #3498db; color: white; }
        .user-managed { background: #ecf0f1; color: #2c3e50; border: 2px dashed #bdc3c7; }
        
        .comparison-table {
            width: 100%;
            border-collapse: collapse;
            margin: 30px 0;
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .comparison-table th {
            background: linear-gradient(45deg, #2c3e50, #34495e);
            color: white;
            padding: 15px;
            text-align: left;
            font-weight: 600;
        }
        
        .comparison-table td {
            padding: 12px 15px;
            border-bottom: 1px solid #ecf0f1;
        }
        
        .comparison-table tr:hover {
            background: #f8f9fa;
        }
        
        .providers-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        
        .provider-card {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 20px;
            border-radius: 10px;
            border-left: 5px solid #3498db;
            transition: all 0.3s ease;
        }
        
        .provider-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        
        .provider-name {
            font-size: 1.2em;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .service-list {
            list-style: none;
            padding: 0;
        }
        
        .service-list li {
            padding: 3px 0;
            color: #5a6c7d;
        }
        
        .service-list li::before {
            content: "✓ ";
            color: #27ae60;
            font-weight: bold;
        }
        
        .highlight {
            background: linear-gradient(45deg, #74b9ff, #0984e3);
            color: white;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            text-align: center;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }
            
            h1 {
                font-size: 2em;
            }
            
            .models-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>☁️ Cloud Computing Models & Infrastructure Comparison</h1>
        
        <div class="highlight">
            <h3>What is Cloud Computing?</h3>
            <p>Cloud computing delivers computing services over the internet, including servers, storage, databases, networking, software, and analytics. It offers faster innovation, flexible resources, and significant cost savings.</p>
        </div>
        
        <h2>🏗️ Service Models Comparison</h2>
        
        <div class="models-container">
            <div class="model-card traditional">
                <div class="model-title">🏢 Traditional Infrastructure</div>
                <p><strong>You manage everything</strong> - Complete control but maximum responsibility</p>
                <div class="stack-diagram">
                    <div class="stack-layer user-managed">Applications</div>
                    <div class="stack-layer user-managed">Data</div>
                    <div class="stack-layer user-managed">Runtime</div>
                    <div class="stack-layer user-managed">Middleware</div>
                    <div class="stack-layer user-managed">OS</div>
                    <div class="stack-layer user-managed">Virtualization</div>
                    <div class="stack-layer user-managed">Servers</div>
                    <div class="stack-layer user-managed">Storage</div>
                    <div class="stack-layer user-managed">Networking</div>
                </div>
                <p><strong>Use Case:</strong> High compliance requirements, complete control needed</p>
            </div>
            
            <div class="model-card iaas">
                <div class="model-title">🖥️ IaaS (Infrastructure as a Service)</div>
                <p><strong>Rent IT infrastructure</strong> - Virtual machines, storage, networks on-demand</p>
                <div class="stack-diagram">
                    <div class="stack-layer user-managed">Applications</div>
                    <div class="stack-layer user-managed">Data</div>
                    <div class="stack-layer user-managed">Runtime</div>
                    <div class="stack-layer user-managed">Middleware</div>
                    <div class="stack-layer user-managed">OS</div>
                    <div class="stack-layer managed">Virtualization</div>
                    <div class="stack-layer managed">Servers</div>
                    <div class="stack-layer managed">Storage</div>
                    <div class="stack-layer managed">Networking</div>
                </div>
                <p><strong>Examples:</strong> AWS EC2, Azure VMs, Google Compute Engine</p>
                <p><strong>Use Case:</strong> Custom applications, testing environments, backup solutions</p>
            </div>
            
            <div class="model-card paas">
                <div class="model-title">⚙️ PaaS (Platform as a Service)</div>
                <p><strong>Development platform</strong> - Build, test, deploy apps without managing infrastructure</p>
                <div class="stack-diagram">
                    <div class="stack-layer user-managed">Applications</div>
                    <div class="stack-layer user-managed">Data</div>
                    <div class="stack-layer managed">Runtime</div>
                    <div class="stack-layer managed">Middleware</div>
                    <div class="stack-layer managed">OS</div>
                    <div class="stack-layer managed">Virtualization</div>
                    <div class="stack-layer managed">Servers</div>
                    <div class="stack-layer managed">Storage</div>
                    <div class="stack-layer managed">Networking</div>
                </div>
                <p><strong>Examples:</strong> Heroku, AWS Elastic Beanstalk, Google App Engine</p>
                <p><strong>Use Case:</strong> Rapid development, web applications, API development</p>
            </div>
            
            <div class="model-card saas">
                <div class="model-title">📱 SaaS (Software as a Service)</div>
                <p><strong>Ready-to-use software</strong> - Access applications via web browser</p>
                <div class="stack-diagram">
                    <div class="stack-layer managed">Applications</div>
                    <div class="stack-layer managed">Data</div>
                    <div class="stack-layer managed">Runtime</div>
                    <div class="stack-layer managed">Middleware</div>
                    <div class="stack-layer managed">OS</div>
                    <div class="stack-layer managed">Virtualization</div>
                    <div class="stack-layer managed">Servers</div>
                    <div class="stack-layer managed">Storage</div>
                    <div class="stack-layer managed">Networking</div>
                </div>
                <p><strong>Examples:</strong> Gmail, Salesforce, Microsoft 365, Slack</p>
                <p><strong>Use Case:</strong> End-user applications, collaboration tools, CRM systems</p>
            </div>
        </div>
        
        <h2>📊 Detailed Comparison</h2>
        
        <table class="comparison-table">
            <thead>
                <tr>
                    <th>Aspect</th>
                    <th>Traditional</th>
                    <th>IaaS</th>
                    <th>PaaS</th>
                    <th>SaaS</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><strong>Control Level</strong></td>
                    <td>Complete control</td>
                    <td>High control</td>
                    <td>Medium control</td>
                    <td>Limited control</td>
                </tr>
                <tr>
                    <td><strong>Management Responsibility</strong></td>
                    <td>Everything</td>
                    <td>OS, Apps, Data</td>
                    <td>Apps, Data only</td>
                    <td>Data only</td>
                </tr>
                <tr>
                    <td><strong>Setup Time</strong></td>
                    <td>Months</td>
                    <td>Hours/Days</td>
                    <td>Minutes/Hours</td>
                    <td>Immediate</td>
                </tr>
                <tr>
                    <td><strong>Scalability</strong></td>
                    <td>Limited & slow</td>
                    <td>High & fast</td>
                    <td>Automatic</td>
                    <td>Provider managed</td>
                </tr>
                <tr>
                    <td><strong>Cost Model</strong></td>
                    <td>High upfront CapEx</td>
                    <td>Pay-as-you-use</td>
                    <td>Subscription based</td>
                    <td>Per user/feature</td>
                </tr>
                <tr>
                    <td><strong>Maintenance</strong></td>
                    <td>Full responsibility</td>
                    <td>Infrastructure managed</td>
                    <td>Platform managed</td>
                    <td>Fully managed</td>
                </tr>
            </tbody>
        </table>
        
        <h2>🌐 Major Cloud Providers & Offerings</h2>
        
        <div class="providers-grid">
            <div class="provider-card">
                <div class="provider-name">🟨 Amazon Web Services (AWS)</div>
                <ul class="service-list">
                    <li><strong>IaaS:</strong> EC2, S3, VPC, RDS</li>
                    <li><strong>PaaS:</strong> Elastic Beanstalk, Lambda</li>
                    <li><strong>SaaS:</strong> WorkDocs, WorkMail</li>
                    <li><strong>Market Share:</strong> ~32%</li>
                    <li><strong>Strengths:</strong> Mature, extensive services</li>
                </ul>
            </div>
            
            <div class="provider-card">
                <div class="provider-name">🔷 Microsoft Azure</div>
                <ul class="service-list">
                    <li><strong>IaaS:</strong> Virtual Machines, Storage, Networking</li>
                    <li><strong>PaaS:</strong> App Service, Functions</li>
                    <li><strong>SaaS:</strong> Microsoft 365, Dynamics</li>
                    <li><strong>Market Share:</strong> ~23%</li>
                    <li><strong>Strengths:</strong> Enterprise integration, hybrid</li>
                </ul>
            </div>
            
            <div class="provider-card">
                <div class="provider-name">🔴 Google Cloud Platform</div>
                <ul class="service-list">
                    <li><strong>IaaS:</strong> Compute Engine, Cloud Storage</li>
                    <li><strong>PaaS:</strong> App Engine, Cloud Functions</li>
                    <li><strong>SaaS:</strong> Workspace, Analytics</li>
                    <li><strong>Market Share:</strong> ~10%</li>
                    <li><strong>Strengths:</strong> AI/ML, data analytics</li>
                </ul>
            </div>
            
            <div class="provider-card">
                <div class="provider-name">🅰️ Alibaba Cloud</div>
                <ul class="service-list">
                    <li><strong>IaaS:</strong> ECS, Object Storage</li>
                    <li><strong>PaaS:</strong> Function Compute</li>
                    <li><strong>SaaS:</strong> DingTalk, AliCloud Mail</li>
                    <li><strong>Market Share:</strong> ~6%</li>
                    <li><strong>Strengths:</strong> Asia-Pacific market leader</li>
                </ul>
            </div>
            
            <div class="provider-card">
                <div class="provider-name">🌊 IBM Cloud</div>
                <ul class="service-list">
                    <li><strong>IaaS:</strong> Virtual Servers, Block Storage</li>
                    <li><strong>PaaS:</strong> Cloud Foundry, OpenShift</li>
                    <li><strong>SaaS:</strong> Watson AI, Security services</li>
                    <li><strong>Market Share:</strong> ~4%</li>
                    <li><strong>Strengths:</strong> Enterprise, AI/Watson</li>
                </ul>
            </div>
            
            <div class="provider-card">
                <div class="provider-name">⚡ Oracle Cloud</div>
                <ul class="service-list">
                    <li><strong>IaaS:</strong> Compute, Storage, Networking</li>
                    <li><strong>PaaS:</strong> Database Cloud, Integration</li>
                    <li><strong>SaaS:</strong> ERP, HCM, CRM applications</li>
                    <li><strong>Market Share:</strong> ~2%</li>
                    <li><strong>Strengths:</strong> Database, enterprise apps</li>
                </ul>
            </div>
        </div>
        
        <div class="highlight">
            <h3>🎯 Choosing the Right Model</h3>
            <p><strong>Traditional:</strong> High compliance, complete control needed<br>
            <strong>IaaS:</strong> Custom applications, development flexibility<br>
            <strong>PaaS:</strong> Rapid development, focus on applications<br>
            <strong>SaaS:</strong> Ready-to-use solutions, minimal IT overhead</p>
        </div>
        
        <h2>💡 Key Benefits of Cloud Computing</h2>
        
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
            <div style="background: #e8f5e8; padding: 15px; border-radius: 8px; border-left: 4px solid #27ae60;">
                <strong>💰 Cost Efficiency</strong><br>
                Reduce capital expenses, pay only for what you use
            </div>
            <div style="background: #e8f4fd; padding: 15px; border-radius: 8px; border-left: 4px solid #3498db;">
                <strong>📈 Scalability</strong><br>
                Scale resources up or down based on demand
            </div>
            <div style="background: #fef7e8; padding: 15px; border-radius: 8px; border-left: 4px solid #f39c12;">
                <strong>⚡ Speed & Agility</strong><br>
                Deploy applications faster than traditional methods
            </div>
            <div style="background: #fce8e8; padding: 15px; border-radius: 8px; border-left: 4px solid #e74c3c;">
                <strong>🔧 Maintenance</strong><br>
                Reduced IT maintenance and management overhead
            </div>
        </div>
    </div>
</body>
</html>
