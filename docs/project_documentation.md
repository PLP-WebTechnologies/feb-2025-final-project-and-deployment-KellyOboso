// Mobile Navigation Toggle
const hamburger = document.querySelector('.hamburger');
const navLinks = document.querySelector('.nav-links');
if (hamburger && navLinks) {
    hamburger.addEventListener('click', () => {
        navLinks.classList.toggle('active');
        hamburger.classList.toggle('active');
    });

    // Close mobile menu when clicking outside
    document.addEventListener('click', (e) => {
        if (!hamburger.contains(e.target) && !navLinks.contains(e.target)) {
            navLinks.classList.remove('active');
            hamburger.classList.remove('active');
        }
    });
}

// Enhanced Contact Form Interactivity and Validation
const contactForm = document.getElementById('contact-form');
if (contactForm) {
    const formInputs = contactForm.querySelectorAll('input, select, textarea');

    // Helper: Show inline message
    function showFormMessage(type, message) {
        let msg = contactForm.querySelector('.form-message');
        if (!msg) {
            msg = document.createElement('div');
            msg.className = 'form-message';
            contactForm.insertBefore(msg, contactForm.firstChild);
        }
        msg.textContent = message;
        msg.className = 'form-message ' + type;
        msg.style.display = 'block';
        setTimeout(() => { msg.style.display = 'none'; }, 4000);
    }

    // Simple validation
    function validateSimple(input) {
        if (input.type === 'email') {
            return input.value.trim().match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/);
        }
        if (input.type === 'tel') {
            return input.value.trim().length >= 7;
        }
        if (input.tagName === 'SELECT') {
            return input.value !== '';
        }
        if (input.type === 'checkbox') {
            return true;
        }
        return input.value.trim().length > 0;
    }

    contactForm.addEventListener('submit', function(e) {
        e.preventDefault();
        let valid = true;
        formInputs.forEach(input => {
            if (input.hasAttribute('required') && !validateSimple(input)) {
                valid = false;
                input.style.borderColor = 'var(--error-color)';
            } else {
                input.style.borderColor = 'var(--success-color)';
            }
        });
        if (valid) {
            showFormMessage('success', 'Thank you! Your message has been sent. We will get back to you soon.');
            contactForm.reset();
        } else {
            showFormMessage('error', 'Please fill in all required fields correctly.');
        }
    });
}

// Add styles for form-message
const formMsgStyle = document.createElement('style');
formMsgStyle.textContent = `
.form-message {
    margin-bottom: 1rem;
    padding: 1rem;
    border-radius: 4px;
    font-size: 1rem;
    text-align: center;
    display: none;
}
.form-message.success {
    background: var(--success-color);
    color: #fff;
}
.form-message.error {
    background: var(--error-color);
    color: #fff;
}
`;
document.head.appendChild(formMsgStyle);

// Enhanced Newsletter Form Interactivity
const newsletterForm = document.getElementById('newsletter-form');
if (newsletterForm) {
    const emailInput = newsletterForm.querySelector('input[type="email"]');
    function showNewsletterMsg(type, message) {
        let msg = newsletterForm.querySelector('.newsletter-message');
        if (!msg) {
            msg = document.createElement('div');
            msg.className = 'newsletter-message';
            newsletterForm.insertBefore(msg, newsletterForm.firstChild);
        }
        msg.textContent = message;
        msg.className = 'newsletter-message ' + type;
        msg.style.display = 'block';
        setTimeout(() => { msg.style.display = 'none'; }, 4000);
    }

    newsletterForm.addEventListener('submit', function(e) {
        e.preventDefault();
        const email = emailInput.value.trim();
        if (!email) {
            showNewsletterMsg('error', 'Please enter your email address.');
            emailInput.style.borderColor = 'var(--error-color)';
            return;
        }
        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
            showNewsletterMsg('error', 'Please enter a valid email address.');
            emailInput.style.borderColor = 'var(--error-color)';
            return;
        }
        showNewsletterMsg('success', 'Thank you for subscribing to our newsletter!');
        emailInput.style.borderColor = 'var(--success-color)';
        newsletterForm.reset();
    });

    emailInput.addEventListener('input', function() {
        emailInput.style.borderColor = '';
    });
}

// Add styles for newsletter-message
const newsletterMsgStyle = document.createElement('style');
newsletterMsgStyle.textContent = `
.newsletter-message {
    margin-bottom: 1rem;
    padding: 0.75rem 1rem;
    border-radius: 4px;
    font-size: 1rem;
    text-align: center;
    display: none;
}
.newsletter-message.success {
    background: var(--success-color);
    color: #fff;
}
.newsletter-message.error {
    background: var(--error-color);
    color: #fff;
}
`;
document.head.appendChild(newsletterMsgStyle);

// Notification system
function showNotification(message, type = 'success') {
    const notification = document.createElement('div');
    notification.className = `notification ${type}`;
    notification.textContent = message;
    
    document.body.appendChild(notification);
    
    // Trigger animation
    setTimeout(() => {
        notification.classList.add('show');
    }, 100);
    
    // Remove notification after 3 seconds
    setTimeout(() => {
        notification.classList.remove('show');
        setTimeout(() => {
            notification.remove();
        }, 300);
    }, 3000);
}

// Smooth scrolling for anchor links
document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function (e) {
        e.preventDefault();
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
            target.scrollIntoView({
                behavior: 'smooth'
            });
        }
    });
});

// Add animation to route cards on scroll
const observerOptions = {
    threshold: 0.1
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('animate');
        }
    });
}, observerOptions);

document.querySelectorAll('.route-card').forEach(card => {
    observer.observe(card);
});

// Add loading animation to images
document.querySelectorAll('img').forEach(img => {
    img.addEventListener('load', function() {
        this.classList.add('loaded');
    });
});

// Route selection handling
const routeSelect = document.getElementById('route');
if (routeSelect) {
    routeSelect.addEventListener('change', (e) => {
        const selectedRoute = e.target.value;
        if (selectedRoute) {
            // You could fetch route details here
            console.log(`Selected route: ${selectedRoute}`);
        }
    });
}

// Add CSS for notifications
const style = document.createElement('style');
style.textContent = `
    .notification {
        position: fixed;
        top: 20px;
        right: 20px;
        padding: 1rem 2rem;
        border-radius: 4px;
        color: white;
        transform: translateX(120%);
        transition: transform 0.3s ease;
        z-index: 1000;
    }
    
    .notification.show {
        transform: translateX(0);
    }
    
    .notification.success {
        background-color: var(--success-color);
    }
    
    .notification.error {
        background-color: var(--error-color);
    }
`;
document.head.appendChild(style);

document.addEventListener('DOMContentLoaded', function () {
    // Sidebar Collapse/Expand
    const sidebar = document.querySelector('.sidebar');
    const collapseBtn = document.getElementById('collapseSidebar');
    const mainContent = document.querySelector('.main-content');

    // Restore sidebar state from localStorage
    if (sidebar) {
        const collapsed = localStorage.getItem('busconnect-sidebar-collapsed');
        if (collapsed === 'true') {
            sidebar.classList.add('collapsed');
        } else {
            sidebar.classList.remove('collapsed');
        }
    }

    if (collapseBtn && sidebar) {
        collapseBtn.addEventListener('click', () => {
            sidebar.classList.toggle('collapsed');
            // Save state to localStorage
            localStorage.setItem('busconnect-sidebar-collapsed', sidebar.classList.contains('collapsed'));
        });
    }

    // Sidebar Mobile Toggle (optional for mobile)
    function openSidebar() {
        if (sidebar && mainContent) {
            sidebar.classList.add('open');
            mainContent.classList.add('sidebar-open');
        }
    }
    function closeSidebar() {
        if (sidebar && mainContent) {
            sidebar.classList.remove('open');
            mainContent.classList.remove('sidebar-open');
        }
    }

    // Dark/Light Mode Toggle
    const darkModeToggle = document.getElementById('darkModeToggle');
    const root = document.documentElement;

    function setDarkMode(enabled) {
        if (enabled) {
            root.classList.add('dark-mode');
            document.body.classList.add('dark-mode');
            localStorage.setItem('busconnect-dark-mode', 'true');
            if (darkModeToggle) darkModeToggle.innerHTML = '<i class="fas fa-sun"></i>';
        } else {
            root.classList.remove('dark-mode');
            document.body.classList.remove('dark-mode');
            localStorage.setItem('busconnect-dark-mode', 'false');
            if (darkModeToggle) darkModeToggle.innerHTML = '<i class="fas fa-moon"></i>';
        }
    }

    if (darkModeToggle) {
        darkModeToggle.addEventListener('click', () => {
            const isDark = root.classList.contains('dark-mode');
            setDarkMode(!isDark);
        });
    }

    // On load, set dark mode from localStorage
    const darkPref = localStorage.getItem('busconnect-dark-mode');
    if (darkPref === 'true') setDarkMode(true);
    else setDarkMode(false);
}); 
