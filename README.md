<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Handyman Services - Book a Job</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 800px; margin: 0 auto; padding: 20px; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; }
        input, textarea, select { width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box; }
        button { background: #007cba; color: white; padding: 12px 30px; border: none; border-radius: 4px; cursor: pointer; font-size: 16px; }
        button:hover { background: #005a87; }
        .services { background: #f5f5f5; padding: 20px; border-radius: 8px; margin: 20px 0; }
    </style>
</head>
<body>
    <h1>Handyman Services NZ</h1>
    
    <div class="services">
        <h2>Services</h2>
        <ul>
            <li>Plumbing repairs</li>
            <li>Electrical (minor)</li>
            <li>General maintenance</li>
            <li>Assembly & installation</li>
        </ul>
    </div>

    <h2>Request a Quote</h2>
    <form id="contactForm">
        <div class="form-group">
            <label>Name *</label>
            <input type="text" id="name" required>
        </div>
        
        <div class="form-group">
            <label>Email or Phone *</label>
            <input type="text" id="contact" required>
        </div>
        
        <div class="form-group">
            <label>Service needed *</label>
            <select id="service" required>
                <option value="">Select service</option>
                <option>Plumbing</option>
                <option>Electrical</option>
                <option>General maintenance</option>
                <option>Assembly</option>
                <option>Other</option>
            </select>
        </div>
        
        <div class="form-group">
            <label>Job description *</label>
            <textarea id="message" rows="5" required placeholder="Describe the job..."></textarea>
        </div>
        
        <div class="form-group">
            <label>Preferred date</label>
            <input type="date" id="date">
        </div>
        
        <button type="submit">Send Request</button>
    </form>

    <script>
        document.getElementById('contactForm').addEventListener('submit', function(e) {
            e.preventDefault();
            alert('Form submitted! (Demo mode - will connect to EmailJS next)');
        });
    </script>
</body>
</html>
