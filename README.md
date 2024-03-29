<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Отзывы o продуктах</title>
  <style>
    body {
      font-family: Arial, sans-serif;
    }

    .review-form, .product-list {
      margin: 20px;
    }

    .product {
      cursor: pointer;
      margin-bottom: 10px;
    }

    .reviews {
      margin-top: 10px;
    }

    .delete-btn {
      color: red;
      cursor: pointer;
      margin-left: 10px;
    }
  </style>
</head>
<body>

  <div class="review-form">
    <h2>Добавить отзыв</h2>
    <label for="productName">Название продукта:</label>
    <input type="text" id="productName" required>
    <br>
    <label for="reviewText">Текст отзыва:</label>
    <textarea id="reviewText" rows="4" required></textarea>
    <br>
    <button onclick="addReview()">Добавить отзыв</button>
  </div>

  <div class="product-list">
    <h2>Список продуктов</h2>
    <div id="products"></div>
  </div>

  <div id="reviews" class="reviews"></div>

  <script>
    function addReview() {
      const productName = document.getElementById('productName').value;
      const reviewText = document.getElementById('reviewText').value;

      if (productName && reviewText) {
        const review = { productName, reviewText };
        let reviews = JSON.parse(localStorage.getItem('reviews')) || [];
        reviews.push(review);
        localStorage.setItem('reviews', JSON.stringify(reviews));

        document.getElementById('productName').value = '';
        document.getElementById('reviewText').value = '';

        showProducts();
      } else {
        alert('Пожалуйста, заполните все поля.');
      }
    }

    function showProducts() {
      const productsDiv = document.getElementById('products');
      const reviews = JSON.parse(localStorage.getItem('reviews')) || [];
      const products = Array.from(new Set(reviews.map(review => review.productName)));

      productsDiv.innerHTML = '';

      products.forEach(product => {
        const productDiv = document.createElement('div');
        productDiv.classList.add('product');
        productDiv.textContent = product;
        productDiv.onclick = () => showReviews(product);
        productsDiv.appendChild(productDiv);
      });
    }

    function showReviews(product) {
  const reviewsDiv = document.getElementById('reviews');
  const reviews = JSON.parse(localStorage.getItem('reviews')) || [];
  const productReviews = reviews.filter(review => review.productName === product);

  reviewsDiv.innerHTML = '';

  productReviews.forEach((review, index) => {
    const reviewDiv = document.createElement('div');
    reviewDiv.innerHTML = `<strong>${review.productName}:</strong>${review.reviewText}`; 
    const deleteBtn = document.createElement('span');
    deleteBtn.classList.add('delete-btn');
    deleteBtn.textContent = 'Удалить';
    deleteBtn.onclick = () => deleteReview(index);
    reviewDiv.appendChild(deleteBtn);
    reviewsDiv.appendChild(reviewDiv);
  });
}

    function deleteReview(index) {
      let reviews = JSON.parse(localStorage.getItem('reviews')) || [];
      reviews.splice(index, 1);
      localStorage.setItem('reviews', JSON.stringify(reviews));
      showProducts();
      showReviews(document.querySelector('.product').textContent);
    }


    showProducts();
  </script>

</body>
</html>
