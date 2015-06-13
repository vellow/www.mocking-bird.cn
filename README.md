A live data generator, RESTful mock server, Allow Cross-site HTTP request generated json data
www.mocking-bird.cn

### Documents
        index: function(format) {
            return _self['parentIdx']
        },
        firstName: function(gender) {
            _self.gender = gender || (this.bool() ? "male" : "female");
            _self.firstName = dict.getItem(_self.gender + "FirstNames");
            return _self.firstName
        },
        surname: function() {
            _self.surname = dict.getItem("surnames");
            return _self.surname
        },
        gender: function() {
            return _self.gender || (this.bool() ? "male" : "female")
        },
        company: function() {
            return _self.company = dict.getItem("companies")
        },
        countriesList: function() {
            var s = function(a, b) {
                    return a.name < b.name ? -1 : a.name > b.name ? 1 : 0
                };
            var countries = dict.getField("countries").slice(0).sort(s);
            var result = {};
            for (var i = 0, l=countries.length; i < l; i++) {
                    result[countries[i].abbr] = countries[i].name
            }
            return result
        },
        country: function(isAbbr) {
            return dict.getItem("countries")[isAbbr ? "abbr" : "name"]
        },
        city: function() {
            return dict.getItem("cities")
        },
        state: function(isAbbr) {
            return dict.getItem("states")[isAbbr ? "abbr" : "name"]
        },
        street: function() {
            return dict.getItem("streets")
        },
        floating: function(left, right, digits, format) {
                    left = left || 0, right = right || 10;
                    digits = digits || (left.toString().split(".")[1] || []).length || 4;
                    var weight = Math.pow(10, digits),
                        wResult = this.integer(left * weight, right * weight),
                        result = (wResult / weight).toFixed(digits);
                    return format ? numeral(result).format(format) : parseFloat(result)
                },
        integer: function(left, right, format) {
                    left = left || 0, right = right || 10;
                    var result = Math.round(left - .5 + Math.random() * (right - left + 1));
                    return format ? numeral(result).format(format) : result
                },
        random: function() {
            var a = [].slice.call(arguments);
            return a[this.integer(0, a.length - 1)]
        },
        bool: function() {
            return !!Math.floor(2 * Math.random())
        },
        objectId: function() {
            var timeSecond = ((new Date).getTime() / 1e3 | 0).toString(16);
            return timeSecond + "xxxxxxxxxxxxxxxx".replace(/x/g, function() {
                return (16 * Math.random() | 0).toString(16)
            }).toLowerCase()
        },
        guid: function() {
            var tpl = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx";
            return tpl.replace(/[xy]/g, function(p) {
                var num = 16 * Math.random() | 0,
                    result = "x" === p ? num : 3 & num | 8;
                return result.toString(16)
            })
        },
        email: function(isRandom) {
            var name = _self.firstName || this.firstName(),
                surname = _self.surname || this.surname(),
                company = _self.company || this.company();
            return isRandom && (name = this.firstName(), surname = this.surname(), company = this.company()), (name + surname + "@" + company + ".com").toLowerCase()
        },
        phone: function(format) {
            var phoneNum = '0' + this.integer(10, 999) + this.integer(1e3, 9999),
                i = 0;
            format = format || "(xxx) xxxx-xxxx";

            return format.replace(/x/g, function() {
                return phoneNum.charAt(i++)
            })
        },
        date: function(leftDate, rightDate, format) {
            leftDate = leftDate || new Date(1970, 0, 1), rightDate = rightDate || new Date;
            var result = new Date(leftDate.getTime() + Math.random() * (rightDate.getTime() - leftDate.getTime()));
            return format ? dateFormat.format.date(result, format) : result
        },
        /*
        ** 1w, 1 words
        *  2s, 2 sentence
        *  3p, 3 paragraph
         */
        lorem: function(query) {
            return lorem(query)
        }
    };
