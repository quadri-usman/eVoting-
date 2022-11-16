body: JSON.stringify({
number: 12345678909,
}),

passport.use(
"local.signup",
new localStrategy(User.authenticate(), {
usernameField: "email",
passwordField: "password",
passReqToCallback: true,
})
);

(req, email, password, done) => {
//All validation logic comes here
let password1 = req.body.password;
let password2 = req.body.password2;
req.checkBody("email", "Invalid email").notEmpty().isEmail();
req
.checkBody("password", "Password must be more than 4 characters")
.not()
.isEmpty()
.isLength({ min: 4 });
let errors = req.validationErrors();
//just to see the output in your console
console.log(password1);
console.log(password2);
if (errors) {
let messages = [];
errors.forEach(function (error) {
messages.push(error.msg);
});
return done(null, false, req.flash("error", messages));
} else if (password1 !== password2) {
return done(
null,
false,
req.flash("error", "Password and Confirm Password must match")
);
}
};
