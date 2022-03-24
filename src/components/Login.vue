<template>
    

    <div class="container">
        <div class="row justify-content-center bg-success" >
            <div class="col-xl-10 col-lg-12 col-md-9 bg-success">
                <div class="card o-hidden border-0 shadow-lg my-5 bg-success">
                    <div class="card-body p-0 bg-success">
                        <div class="row bg-gradient-dark d-flex justify-content-center align-items-center" >
                             
                            <div class="col-lg-6 d-none d-lg-block bg-login-image min-vh-100">
                                <!-- <img src="src/assets/img/login.png" alt="" max="2000" width="500"> -->
                            </div>
                            
                            <div class="col-lg-6">
                                <div class="p-5">
                                    <div class="text-center">
                                        <h1 class="text-light mb-4">Login</h1>
                                    </div>

                                    <!-- @if ($errors->any())
                                        <div class="alert alert-danger border-left-danger" role="alert">
                                            <ul class="pl-4 my-2">
                                                @foreach ($errors->all() as $error)
                                                    <li>{{ $error }}</li>
                                                @endforeach
                                            </ul>
                                        </div>
                                    @endif -->                                
                                    <form v-on:submit.prevent="login" method="post">
                                        <!-- <input type="hidden" name="_token" value="{{ csrf_token() }}"> -->

                                        <div class="form-group">
                                            <input v-model="username" class="form-control " id="username" type="text" placeholder="Username"/>
                                            <label for="username" class="text-light">Username</label>
                                        </div>

                                        <div class="form-group">
                                            <input v-model="password" class="form-control" id="password" type="password" placeholder="Password"/>
                                            <label for="password" class="text-light">Password</label>
                                        </div>

                                        <!-- <div class="form-group">
                                            <div class="custom-control custom-checkbox small">
                                                <input type="checkbox" class="custom-control-input" name="remember" id="remember" {{ old('remember') ? 'checked' : '' }}>
                                                <label class="custom-control-label" for="remember">{{ __('Remember Me') }}</label>
                                            </div>
                                        </div> -->

                                        <div class="form-group">
                                            <input type="submit" class="btn btn-success" value="Login">
                                        </div>

                                        <hr>

                                        <!-- <div class="form-group">
                                            <button type="button" class="btn btn-github btn-user btn-block">
                                                <i class="fab fa-github fa-fw"></i> {{ __('Login with GitHub') }}
                                            </button>
                                        </div>

                                        <div class="form-group">
                                            <button type="button" class="btn btn-twitter btn-user btn-block">
                                                <i class="fab fa-twitter fa-fw"></i> {{ __('Login with Twitter') }}
                                            </button>
                                        </div>

                                        <div class="form-group">
                                            <button type="button" class="btn btn-facebook btn-user btn-block">
                                                <i class="fab fa-facebook-f fa-fw"></i> {{ __('Login with Facebook') }}
                                            </button>
                                        </div>
                                    </form> -->

                                    <!-- <hr>

                                    @if (Route::has('password.request'))
                                        <div class="text-center">
                                            <a class="small" href="{{ route('password.request') }}">
                                                {{ __('Forgot Password?') }}
                                            </a>
                                        </div>
                                    @endif

                                    @if (Route::has('register'))
                                        <div class="text-center">
                                            <a class="small" href="{{ route('register') }}">{{ __('Create an Account!') }}</a>
                                        </div>
                                    @endif -->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
module.exports = {
    data: function() {
        return {
            username: "",
            password: "",
            message: "",
            // message: "Loading...",
            // type : 'secondary',
            // show : false,
            // spin: false
        }
    },
    methods: {
        login: function(){
            // this.show = true;
            // this.spin = true;

            // alert("username anda: " + username, "password anda: " + password)
            this.message = "Wait a second..."; // alert notif
            // console.log(this.message);

            // mapping data
            let form = {
                "username": this.username,
                "password": this.password
            }
            axios.post(base_url + '/login', form)
            .then(response => {
                if(response.data.success ==  true){
                    this.message = response.data.message; //alert

                    // menyimpan token ke cookies
                    // cek apakah token sudah ada di cookies?
                    if(this.$cookies.isKey('Authorization')){
                        this.$cookies.remove('Authorization'); // jika ada dihapus
                    }

                    // menyimpan token ke cookies
                    this.$cookies.set('Authorization', response.data.data.token);
                    // this.type = "success";
                    // this.message = response.data.message;
                    // this.componentName = "app";
                    location.reload();
                } else {
                    // this.type = "danger";
                    this.message = response.data.message;
                }
            })
            // .catch(error => {
            // console.log(error);
            // });
        },
    }
}
</script>