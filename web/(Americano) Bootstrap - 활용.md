# Bootstrap

[BootstrapDocs](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

### Components

##### Alerts

```css
<h2>Alerts</h2>
<div class="alert alert-primary" role="alert">
  A simple primary alert—check it out!
</div>
<div class="alert alert-secondary" role="alert">
  A simple secondary alert—check it out!
</div>
```

##### Badge

```css
<h2>Badges</h2>
<h4>Example heading <span class="badge bg-secondary">New</span></h4>
<button type="button" class="btn btn-primary">
  Notifications <span class="badge bg-secondary">4</span>
</button>
<span class="badge bg-success">Success</span>
<span class="badge bg-info text-dark">Info</span>
<span class="badge rounded-pill bg-danger">Danger</span>
<span class="badge rounded-pill bg-warning text-dark">Warning</span>
<span class="badge rounded-pill bg-info text-dark">Info</span>
```

##### Buttons

```css
<button type="button" class="btn btn-outline-primary btn-lg">Primary</button>
<button type="button" class="btn btn-outline-secondary btn-sm">Secondary</button>
<button type="button" class="btn btn-outline-success" disabled>Success</button>
<button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
```

##### ✔ Card

```css
<div class="card" style="width: 18rem;">
  <img src="..." class="card-img-top" alt="...">
  <div class="card-body">
    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
  </div>
</div>
```

##### ✔ Carousel

```css
<h2>Carousel</h2>
<div id="carouselExampleInterval" class="carousel slide" data-bs-ride="carousel">
<div class="carousel-inner">
<div class="carousel-item active" data-bs-interval="10000">
<img src="1.jpg" class="d-block w-100" alt="...">
</div>
<div class="carousel-item" data-bs-interval="2000">
<img src="2.jpg" class="d-block w-100" alt="...">
</div>
<div class="carousel-item">
<img src="3.png" class="d-block w-100" alt="...">
</div>
</div>
<a class="carousel-control-prev" href="#carouselExampleInterval" role="button" data-bs-slide="prev">
<span class="carousel-control-prev-icon" aria-hidden="true"></span>
<span class="visually-hidden">Previous</span>
</a>
<a class="carousel-control-next" href="#carouselExampleInterval" role="button" data-bs-slide="next">
<span class="carousel-control-next-icon" aria-hidden="true"></span>
<span class="visually-hidden">Next</span>
</a>
</div>
```

##### ✔ Modal

```css
<h2>Modal</h2>
    <!-- Button trigger modal -->
    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
      Launch demo modal
    </button>

    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            ...
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>
```

##### Navbar

```css
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Features</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Pricing</a>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

