# FoodieForm-1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Restaurant Review Form</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; padding: 20px; }
        .rating { unicode-bidi: bidi-override; direction: rtl; }
        .rating > span { display: inline-block; position: relative; width: 1.1em; }
        .rating > span:hover:before,
        .rating > span:hover ~ span:before { content: "\2605"; position: absolute; color: gold; }
        textarea { width: 100%; height: 100px; }
        button { background-color: #4CAF50; color: white; padding: 10px 20px; border: none; cursor: pointer; }
    </style>
</head>
<body>
    <h1 id="restaurantName">Restaurant Name</h1>
    <form id="reviewForm">
        <div id="menuItems"></div>
        <h3>Overall Rating</h3>
        <div class="rating">
            <span>☆</span><span>☆</span><span>☆</span><span>☆</span><span>☆</span>
        </div>
        <h3>Comments</h3>
        <textarea id="comments"></textarea>
        <br><br>
        <button type="submit">Submit Review</button>
    </form>

    <script>
        // Function to get URL parameters
        function getParam(name) {
            const urlParams = new URLSearchParams(window.location.search);
            return urlParams.get(name);
        }

        // Set restaurant name and menu items from URL parameters
        document.getElementById('restaurantName').textContent = getParam('restaurant') || 'Restaurant Name';
        const menuItems = (getParam('items') || '').split(',');
        const menuItemsDiv = document.getElementById('menuItems');
        menuItems.forEach(item => {
            if (item) {
                const h3 = document.createElement('h3');
                h3.textContent = item;
                menuItemsDiv.appendChild(h3);
                const rating = document.createElement('div');
                rating.className = 'rating';
                rating.innerHTML = '<span>☆</span><span>☆</span><span>☆</span><span>☆</span><span>☆</span>';
                menuItemsDiv.appendChild(rating);
            }
        });

        // Handle form submission
        document.getElementById('reviewForm').onsubmit = function(e) {
            e.preventDefault();
            const ratings = {};
            document.querySelectorAll('.rating').forEach((rating, index) => {
                const stars = rating.querySelectorAll('span');
                for (let i = stars.length - 1; i >= 0; i--) {
                    if (stars[i].textContent === '★') {
                        ratings[menuItems[index] || 'overall'] = 5 - i;
                        break;
                    }
                }
            });
            const comments = document.getElementById('comments').value;
            const reviewData = {
                restaurant: document.getElementById('restaurantName').textContent,
                ratings: ratings,
                comments: comments
            };
            console.log(reviewData);
            alert('Thank you for your review!');
            // Here you would typically send this data to a server
        };

        // Handle star ratings
        document.querySelectorAll('.rating span').forEach(star => {
            star.onclick = function() {
                let stars = this.parentNode.children;
                for (let i = 0; i < stars.length; i++) {
                    stars[i].textContent = '☆';
                }
                this.textContent = '★';
                let prevSibling = this.previousElementSibling;
                while(prevSibling) {
                    prevSibling.textContent = '★';
                    prevSibling = prevSibling.previousElementSibling;
                }
            };
        });
    </script>
</body>
</html>
