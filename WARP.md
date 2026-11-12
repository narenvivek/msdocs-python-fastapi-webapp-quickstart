# WARP.md - FastAPI Web Application Analysis

## 📋 Project Overview

This is a minimal FastAPI web application designed as a quickstart template for deploying Python web apps to Azure App Service. It demonstrates basic FastAPI functionality with HTML templating and static file serving.

**Project Name:** msdocs-python-fastapi-webapp-quickstart  
**Framework:** FastAPI  
**Template Engine:** Jinja2  
**Deployment Target:** Azure App Service  

## 🏗️ Architecture

### Core Application Structure
```
/Users/naren/msdocs-python-fastapi-webapp-quickstart/
├── main.py                 # Main FastAPI application
├── requirements.txt        # Python dependencies
├── startup.sh             # Production startup script
├── azure.yaml             # Azure Developer CLI config
├── templates/             # Jinja2 HTML templates
│   ├── index.html         # Home page template
│   └── hello.html         # Greeting page template
├── static/               # Static assets
│   ├── favicon.ico       # Site favicon
│   ├── bootstrap/        # Bootstrap CSS framework
│   └── images/           # Image assets
└── infra/               # Infrastructure as Code
    ├── main.bicep       # Azure Bicep template
    ├── main.parameters.json
    └── abbreviations.json
```

## 🔌 API Endpoints

### Routes Overview
The application defines 3 main endpoints:

1. **GET `/`** - Home page
   - Returns HTML response using `index.html` template
   - Displays welcome form for user input

2. **POST `/hello`** - Greeting handler
   - Accepts form data with `name` parameter
   - Returns personalized greeting using `hello.html` template
   - Redirects to home if name is empty

3. **GET `/favicon.ico`** - Favicon handler
   - Serves favicon from static directory
   - Custom headers for proper MIME type

### Key Code Patterns
```python
# Static file mounting
app.mount("/static", StaticFiles(directory="static"), name="static")

# Template configuration
templates = Jinja2Templates(directory="templates")

# Form handling with redirect logic
if name:
    return templates.TemplateResponse('hello.html', {"request": request, 'name':name})
else:
    return RedirectResponse(request.url_for("index"), status_code=status.HTTP_302_FOUND)
```

## 📦 Dependencies

### Python Requirements (`requirements.txt`)
- **fastapi** - Web framework for building APIs
- **uvicorn** - ASGI server for running FastAPI
- **jinja2** - HTML templating engine
- **python-multipart** - For handling form data

### Static Assets
- **Bootstrap 5.1.3** - CSS framework for responsive design
- Custom favicon and Azure branding images

## 🚀 Deployment Configuration

### Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Start development server
uvicorn main:app --reload
```

### Production Deployment
- **Server:** Gunicorn with Uvicorn workers
- **Command:** `gunicorn -w 2 -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8000 main:app`
- **Port:** Configurable via `${PORT:-8000}` environment variable

### Azure App Service Configuration
- **Runtime:** Python 3.13
- **Build:** SCM build during deployment enabled
- **Plan:** B1 tier App Service Plan
- **Auto-deployment:** GitHub Actions workflow configured

## 📋 Infrastructure as Code

### Azure Bicep Template (`infra/main.bicep`)
- **App Service Plan:** B1 SKU for cost-effective hosting
- **App Service:** Python 3.13 runtime with Gunicorn
- **Resource Group:** Auto-generated with environment naming
- **Tags:** Environment-based resource tagging

### Azure Developer CLI Integration
- **Service:** Web service configured in `azure.yaml`
- **Language:** Python
- **Host:** App Service

## 🎨 Frontend Design

### Template Structure
- **Base Framework:** Bootstrap 5.1.3 for responsive design
- **Branding:** Azure logo and corporate styling
- **Security:** Content Security Policy with upgrade-insecure-requests
- **Form Handling:** POST forms with CSRF protection via request context

### UI Components
- Welcome page with name input form
- Personalized greeting page with back navigation
- Consistent Azure branding throughout

## 🔧 Development Patterns

### FastAPI Best Practices
- Async endpoint definitions for better performance
- Proper status code handling (302 for redirects)
- Static file serving with appropriate MIME types
- Request context passed to templates for URL generation

### Error Handling
- Form validation with redirect fallback
- Graceful handling of empty/invalid input
- Proper HTTP status codes for different scenarios

## 📊 Key Metrics & Considerations

### Performance
- **Workers:** 2 Gunicorn workers for parallel request handling
- **Server:** Uvicorn ASGI for high-performance async handling
- **Static Files:** Efficient serving via FastAPI's StaticFiles

### Security
- Content Security Policy headers
- Form data validation
- Secure static file serving

### Scalability
- Containerization ready (startup script provided)
- Azure App Service auto-scaling capabilities
- Stateless design for horizontal scaling

## 🎯 Use Cases

This template is ideal for:
- FastAPI learning and prototyping
- Azure App Service deployment demos
- Simple web applications with form handling
- Bootstrap + FastAPI integration examples
- Infrastructure as Code demonstrations with Bicep

## 📝 Additional Notes

- **GitHub Integration:** Pre-configured workflow for Azure deployment
- **Environment Variables:** Port configuration via environment
- **Logging:** Basic console logging for request tracking
- **Bootstrap Version:** 5.1.3 included locally for offline development
- **Template Engine:** Jinja2 with Azure URL helpers and branding

This project serves as an excellent starting point for FastAPI applications targeting Azure App Service deployment with modern DevOps practices.
