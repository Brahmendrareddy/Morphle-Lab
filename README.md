import os
import subprocess
from django.http import HttpResponse
from datetime import datetime
import pytz

def htop_view(request):
    # Your full name
    full_name = "NUNE BRAHMENDRA REDDY"
    
    # Username (cross-platform compatibility)
    username = os.getenv("USERNAME") or os.getenv("USER") or "Unknown"
    
    # Server time in IST
    ist = pytz.timezone('Asia/Kolkata')
    server_time = datetime.now(ist).strftime('%Y-%m-%d %H:%M:%S.%f %Z')
    
    # Run the 'top' command to get system information (Linux only)
    top_output = subprocess.getoutput("top -b -n 1") if os.name != 'nt' else "Top command not available on Windows."
    
    # Generate HTML response
    response_content = f"""
    <html>
    <body>
        <h1>System Information</h1>
        <p><strong>Name:</strong> {full_name}</p>
        <p><strong>Username:</strong> {username}</p>
        <p><strong>Server Time (IST):</strong> {server_time}</p>
        <h2>Top Output:</h2>
        <pre>{top_output}</pre>
    </body>
    </html>
    """
    return HttpResponse(response_content)
