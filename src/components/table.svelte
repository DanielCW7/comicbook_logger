<script>
    let result;
    try {
        result = fetch('http://localhost:8889/conn.php')
        .then(res => res.json())
        .then(res => console.log(res));
    } catch (error) {
        console.error(error)
    } finally {
        console.log("done")
    }
</script>