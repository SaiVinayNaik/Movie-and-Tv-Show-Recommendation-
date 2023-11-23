#app.py
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

# Dummy user and movie data for demonstration purposes
users = {'user1': 'password1', 'user2': 'password2'}

movies = [
    {'title': 'Movie 1', 'genre': 'Action'},
    {'title': 'Movie 2', 'genre': 'Comedy'},
    {'title': 'Movie 3', 'genre': 'Drama'},
    # Add more movie entries as needed
]

@app.route('/', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        # Process the registration data (store in a database, etc.)
        username = request.form['username']
        password = request.form['password']
        # You can add more processing logic here

        return f'Registration successful! Welcome, {username}. <a href="/login">Login</a>'

    # If the request method is GET, or after processing the POST data
    return render_template('register.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']

        if username in users and users[username] == password:
            return f'Login successful! Welcome back, {username}. <a href="/movies">Go to Movies</a>'
        else:
            return 'Login failed. Please check your username and password.'

    return render_template('login.html')

@app.route('/movies', methods=['GET', 'POST'])
def movie_list():
    if request.method == 'POST':
        # Implement search logic here
        search_query = request.form['search_query'].lower()
        filtered_movies = [movie for movie in movies if search_query in movie['title'].lower() or search_query in movie['genre'].lower()]
        return render_template('movies.html', movies=filtered_movies, search_query=search_query)

    return render_template('movies.html', movies=movies, search_query='')

if __name__ == '__main__':
    app.run(debug=True)
  #movies.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Movies Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #0000FF; /* Light gray background color */
            margin: 0;
        }

        header {
            background-color: #FFA500; /* Blue header background color */
            padding: 10px;
            text-align: center;
        }

        main {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        h2 {
            color: #2c3e50; /* Dark gray text color */
        }

        input[type="text"] {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }

        .movie {
            border-bottom: 1px solid #bdc3c7; /* Light gray border between movies */
            padding: 10px 0;
        }
    </style>
</head>
<body>
    <header>
        <h2>Movie Recommender</h2>
    </header>

    <main>
        <form action="/movies" method="post">
            <label for="search_query">Search:</label>
            <input type="text" id="search_query" name="search_query" placeholder="Search by title or genre">
            <button type="submit">Search</button>
        </form>

        {% if search_query %}
            <p>Showing results for: "{{ search_query }}"</p>
        {% endif %}

        {% for movie in movies %}
            <div class="movie">
                <h3>{{ movie['title'] }}</h3>
                <p>Genre: {{ movie['genre'] }}</p>
            </div>
        {% endfor %}
    </main>
</body>
</html>
#register.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registration Page</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0; /* Light background color */
        }

        form {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }

        input {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }

        button {
            background-color: #4caf50; /* Green */
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <form action="/" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required>

        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required>

        <button type="submit">Register</button>
    </form>
</body>
</html>
#login.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #2ecc71; /* Light green background color */
        }

        form {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }

        input {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }

        button {
            background-color: #3498db; /* Blue */
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <form action="/login" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required>

        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required>

        <button type="submit">Login</button>
    </form>
</body>
</html>
